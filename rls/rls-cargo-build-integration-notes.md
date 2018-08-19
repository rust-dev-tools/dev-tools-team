# RLS <> Cargo <> External build integration

The problem that is being solved is being able to support build systems other than Cargo by the RLS to get diagnostics and project indexing.

## Scope
We can think of the 3 components and boundaries between them:

RLS <> Cargo <> External build system

Who has the responsibility for orchestrating the build?
1. RLS - needs to maintain its own version of dep graph/build plan
2. Cargo - RLS asks Cargo for what needs to be run (e.g. 'given these dirty files, what commands should I rerun') and RLS executes the commands

## Current status
RLS keeps its own crate target dependency graph with commands associated with them. To do so, it invokes Cargo to perform a full `check` build and intercepts every `rustc` invocaton and associates it with a crate target.  When modifying files the RLS detects dirty crate targets and reruns the `rustc` commands.

Goals:
1. Don't duplicate dep graph/build orchestration inside the RLS
2. Understand other build systems - provide a common interface for an executable build plan

## What Cargo does
1. Configuration (incl. package overrides etc.)
2. Version resolution
3. Package dependency graph construction (Cargo.lock)
4. Action graph construction
    1. Feature resolution - may change package deps based on features selected
    2. Lowering dependencies to crate target level (proc macros, bins, libs)
    3. Spliting nodes further into actions (e.g. compile build script, run build script, compile lib)
    4. Differentiate/prune actions based on target dirtiness (fingerprints)
5. Fetching packages (locate on disk, fetch from network)
6. Build plan construction - flattened view into action graph mapped into actual system commands
7. Execute the commands!

## RLS build types
* Incremental build (We assume that action graph has not changed)
  - triggerd by the RLS
  - only requires dirty build plan, which can be executed
* From-scratch build (Action graph might have changed - regenerate it via build system)
  - triggered by the user or the RLS
  - goes through build driver

## Idea: Split Cargo into 3 parts
1. Front (`cargo build` -> action graph)
2. Build orchestration (action graph -> build plan)
3. Execution (invoking the build plan commands)

With this we can mix and match:
1. RLS should always execute the build (we need to intercept the rustc calls to give live diagnostics and save-analysis)
2. External build system can provide its set of packages and dependencies between those while Cargo orchestrates the remaining build into build plan (understandable by the RLS)
3. Cargo can execute its front, fetch packages from crates.io, form an action graph, which can be further reused and lowered into a build plan by the external build system
4. etc.

## Glossary
* Workspace - set of crates that user is actively editing (in pure Cargo world this corresponds to Cargo workspaces)
* Dependencies - supplied by the build system (downloaded from crates.io, specified by path etc, but NOT edited - read-only)
* Build driver - whoever kicks off and controls most of the build (think `cargo build`, `buck build` etc.)
* Package dependency graph - dependencies between concrete packages (e.g. `my-crate 0.3.3` -> `serde 1.0.71`)
* Action graph - graph of abstract actions to be performed, e.g. compiling a crate or running build script
