# Integrating with other build systems
## Idea #1
Allow specifying file outputs to build scripts (BS) in the Cargo manifest (and/or include input files?)
```toml
[build]
outputs = [ "file1", "file2" ]
```

Clearly defines and solves one of the main use cases for build scripts - code generation.

1. If the output files are not specified, always consider BS to be dirty
    * (_good_) correct (but may be inefficient)
    * (_good_) strongly encourages users to specify BS outputs themselves to save rebuild cycles
    * (_bad_) doesn't work well with *modify/prepare external/OS environment* (use cases?)
    * (_bad_) doesn't cover the case where BS outputs and passes down relevant env vars
        * (_solvable?_) Like with current BS output, we could treat generated env vars as a well defined, special-cased file output implicitly needed by all the target kinds
2. Don't allow for `build.outputs` and existing (pre-build script run) package files to overlap
   * (_good_) if we allow that, the interaction is poor - it's possible that build scripts will be considered _fresh_ because the outputs exist before running it but the intention was to _preprocess_ input files
3. (Prior art) Required by numerous build systems:
    * [mozbuild](https://dxr.mozilla.org/mozilla-central/rev/c291143e24019097d087f9307e59b49facaf90cb/python/mozbuild/mozbuild/backend/cargo_build_defs.py) ([context](https://internals.rust-lang.org/t/build-script-capabilities/8635/5?u=xanewok))
    * [Buck](https://buckbuild.com/rule/genrule.html#out) (`genrule.out`)
    * [Bazel](https://docs.bazel.build/versions/master/be/general.html#genrule.outs) (`genrule.outs`)
    * [GN](https://chromium.googlesource.com/chromium/src/tools/gn/+/48062805e19b4697c5fbd926dc649c78b6aaa138/docs/language.md#executing-scripts) (`action.outputs`)

## Idea #2
Restrict generating files only to (Cargo's) `$OUT_DIR`
   * (_good_) doesn't clobber package directory
   * (_good_) `cargo clean` workflow always works as expected
   * (_bad_) restricts possible workflow (preprocessor use cases come to mind, e.g. generate in-tree `${FILE}.i` for each source `${FILE}`)

## Idea #3
Restrict general build script capabilities
* Sandboxed execution
* Compile BS with a Cargo util crate (providing API equivalent to printing `cargo:` keys, allowed access to files etc.)
* Emit BS MIR and only allow execution of a "safe" subset of Rust programs
    * Detect and allow `File::open` and similar APIs only with path literals
        * (_bad_) no `rustc` compile-time errors (diagnostics emitted by Cargo)
        * (_bad_) hardcoded APIs, easy to miss (e.g. `OpenOptions` can also open files)
        * (_bad_) restricts usage, e.g. can't create dynamic `${i}.txt` files

## Idea #4
Solve BS use cases by implementing native/declarative solutions

1. Conditional compilation driven by `rustc` version (solved by [RFC #2523](https://github.com/rust-lang/rfcs/pull/2523), [cuviper/autocfg](https://github.com/cuviper/autocfg) crate)

   1. ([libc](https://github.com/rust-lang/libc/blob/master/build.rs)) - only use case (13k DLs/day)
   2. ([rand](https://github.com/rust-random/rand/blob/master/build.rs)) - only use case (20k DLs/day)
   3. ([serde](https://github.com/serde-rs/serde/blob/master/serde/build.rs)) - only use case (12k DLs/day)
   4. ([regex](https://github.com/rust-lang/regex/blob/master/build.rs)) - only use case (12k DLs/day)
   5. ([num-traits](https://github.com/rust-num/num-traits/blob/master/build.rs)) - only use case (10k DLs/day)
2. Dependence on env vars
    1. `librustc_driver/build.rs`
        1. `CFG_RELEASE`
        2. `CFG_VERSION`
        3. `CFG_VER_DATE`
        4. `CFG_VER_HASH`
    2. `librustc/build.rs`
        1. `CFG_LIBDIR_RELATIVE`
        2. `CFG_COMPILER_HOST_TRIPLE`
        3. `RUSTC_VERIFY_LLVM_IR` (converted into `cfg` flag)
3. feature-specific `cfg` flags
    1. `dlmalloc/build.rs`
        1. `feature = "debug"` -> `cfg` flag `debug_assertions`
4. target-specific `link-lib`
    1. `libunwind/build.rs`
    2. `libstd/build.rs`
    3. `librustc_llvm/build.rs` (partially)
2. Building/linking native dependencies (`*-sys` crates)
   1. [joshtriplett/metadeps](https://github.com/joshtriplett/metadeps) - Run pkg-config from declarative dependencies in Cargo.toml

---
Relevant:
* chromium's disclaimer against `exec_script` rule types
https://chromium.googlesource.com/chromium/src/+/master/.gn#622
