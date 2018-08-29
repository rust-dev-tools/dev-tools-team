# RLS <> Cargo <> External build integration

## Glossary
* Crate - compilation unit as understood by `rustc`
* (Cargo) Package - set of files, including the package manifest that specifies how these can be compiled into crates
* Crate target - Way to compile a package into a crate to be used in a certain purpose, e.g. `bin`, `lib`, `build` (script), `test`, ...
* Crate artifact - file that is the result of crate target compilation
* Workspace - set of crates that user is actively editing (in pure Cargo world this corresponds to Cargo workspaces)
* Build driver - whoever kicks off and controls most of the build (think `cargo build`, `buck build` etc.)
* Package dependency graph - dependencies between concrete packages (e.g. `my-crate 0.3.3` -> `serde 1.0.71`)
* Action graph - graph of abstract actions to be performed, e.g. compiling a crate target or running build script
* Build plan - dependency graph between commands (mostly `rustc` and possibly build plan execution) which, when executed, leads to compilation of (a) crate target(s)

# Problem

The problem that is being solved is twofold:
1. RLS needs to (efficiently) support Rust projects using other build systems than pure Cargo
2. Cargo and other build systems should understand each other


We can identify these 3 components and how they interact with each other:

RLS <> Cargo <> External build system


In general, the interaction between the 3 of these is arbitrary but we decided to linearize it as per above.

Cargo is another build system but adds an abstraction layer - this way RLS would not need to know about other build systems. To do that, we'd need some form of a compatibility middleware that would bridge Cargo and other build systems.

In general, RLS needs to answer the following question: 'given these dirty files, what commands should I run?' to perform its analysis.
For build system interop, Cargo and other build systems need to settle on a common
abstraction (packages/crates/files dependencies? bare invocations?) in order to understand steps needed to compile a target built using the other system.

(RLS) Who should be responsible for build orchestration?
1. RLS internally - needs to maintain its own version of abstract action graph and a mapping between it and a concrete build plan. Needs to understand which crate targets are dirty from a set of dirty files.
2. Build system - RLS fetches the build plan from an external build system. Bonus points if the RLS can also only supply a list of dirty files and receive only the relevant sub-plan to execute.

(Cargo <> other build system) What do systems need to know about each other?
1. Build output for a given crate target - knowing this, a build system can only delegate to the other as a subroutine and use only the resulting artifacts (e.g. `.rlib`, debug symbols)
2. Transitive file-level dependencies
    1. Correct dependency/freshness tracking, e.g. external build system might need to understand that Cargo-managed crate target is fresh and can reuse (cloud) cache-managed artifacts
    2. Generating external build system manifests, e.g. Makefiles
3. Crate target dependencies - (together with file-level deps?) could be used to generate external build system manifests for more abstract build systems, e.g. define Buck/Bazel build rules

# Current status
## RLS
RLS keeps its own crate target dependency graph with commands associated with them (which can be thought of as an action graph + build plan). To do so, it invokes Cargo to perform a full `check` build and intercepts every `rustc` invocaton and associates it with a crate target.  When modifying files the RLS detects dirty crate targets using a path heuristic and reruns the associated `rustc` commands.

We'd like to:
1. Not duplicate dep graph/build orchestration inside the RLS
2. Understand other build systems - provide a common interface for an executable build plan

### Build types
#### Incremental
We assume that the build plan has not changed, which means we can reuse it and query it for required commands that has to be rerun.
  - triggerd by the RLS (source files changed)
#### From scratch
The build plan has possibly changed or is insufficient - we need to regenerate it using the build system.
  - triggered by the RLS (e.g. watched manifest or package configuration changed)
  - triggered by an external build system (explicitly by user)

## Cargo
Cargo is capable of generating a build plan for `cargo build` (with `-Z unstable-options --build-plan`), which is a dependency graph between commands (`rustc` and calling build scripts, together with args, env, cwd), where each command is also tagged with a package name, version, crate target kind, Target/Host architecture differentiation and a list of resulting crate artifacts.

Generating the build plan and its format are currently unstable and not yet used to integrate with other build systems or RLS.

Note: When emitting build plan for `cargo check` is supported, the RLS could possibly drop a dependency the Cargo library. Given a complete build plan, the RLS wouldn't have to start Cargo in-process and execute `rustc` itself on callbacks but just execute the build itself.

### What Cargo does (`cargo {build,check}`)
An arbitrary breakdown that we think might be useful to identify separate, replaceable steps:
1. Configuration (incl. package overrides like `[patch]` etc.)
2. Version resolution (package dependencies are specified as semver *constraints* - this step is responsible for solving those across the entire build plan into specific package versions)
3. Package dependency graph construction (Cargo.lock)
4. Action graph construction
    1. Feature resolution - may change package deps based on features selected
    2. Lowering dependencies to crate target level (proc macros, bins, libs)
    3. Spliting nodes further into actions (e.g. compile build script, run build script, compile lib)
    4. Differentiate/prune actions based on target dirtiness (fingerprints)
5. Fetching packages (locate on disk, fetch from network)
6. Build plan construction - flattened view into action graph mapped into actual system commands
7. Execute the commands!

# Possible plan
## Split Cargo into 3 parts
Having the above sequence of steps for `cargo build`, we can group these into:
1. Front (`cargo build` -> action graph) (Step 1-5)
2. Build orchestration (action graph -> build plan) (Step 6)
3. Execution (invoking the build plan commands) (Step 7)

With this we can mix and match:
1. RLS should always execute the build (needs to invoke `rustc` itself to perform analysis)
2. External build system can provide its set of packages and dependencies between them and Cargo can orchestrate that into a build plan (understandable by the RLS)
3. Cargo can execute its front, fetch packages from crates.io, form an action graph, which can be further reused and translated into a build plan by the external build system
4. etc.

## Concrete goals
### RLS <> other build systems
* Establish a preliminary build plan format (probably use/adapt current Cargo `--build-plan`)
* RLS needs to understand the build plan and be able to execute it, separate from original build system in order to keep it in a consistent state, while consuming the save-analysis and diagnostics
### RLS <> Cargo
* Implement `cargo check ... --build-plan` in Cargo
* Ask Cargo to run the build, but have the RLS execute the steps
### RLS <> Buck
* Buck needs to be able to tell the (almost) exact `rustc` command used to compile a crate target (embed the command in save-analysis?)
* Translate Buck rules/generated save-analysis into build plan
### Cargo <> other build systems
* Establish a preliminary, Cargo action graph format - also notion of package version, targets (incl. build scripts and proc macros), Target/Host arch, Cargo features(?), declarative `rustc` args (profiles)
* Allow Cargo to be fed an action graph format to be later executed by it
* Emit also input files, in addition to command outputs, in the build plan to provide file-level dependencies
* Split Cargo front (Step 1-5) into separate commands to be reused by other build systems
