# Detecting input files for crate
What affects input files?
1. `include*!` built-in macros
2. `#[doc(include = "path")]` attribute
3. Module files
4. (_indirectly_) Extern crates

Points 1 and 2 are easily solvable (resolved during expansion phase - insert dummy nodes and register dependencies).
(https://github.com/Xanewok/rust/tree/dep-info-allow-missing-files)

However, module files and extern crates are tricky.

Firstly, parsing modules can produce additional `include!` directives and create (path) items.
Existence of paths can further drive conditional compilation on `include*!`, `mod MOD;` items etc.
([RFC #2523](https://github.com/rust-lang/rfcs/pull/2523))

Since extern crates obviously import defined paths, the conditional compilation can be driven using those, which means we effectively need to have our dependencies compiled (at least metadata) to be able to resolve the conditional compilation - this is required to know what is `include!`d and so on.

Until [RFC #2523](https://github.com/rust-lang/rfcs/pull/2523) is merged, a package only needs to execute its build script to know its inputs.