Difficulties for translating the rules (Buck -> Cargo)
## `build.rs` (explained in-depth elsewhere)
* missing build outputs
* dependence on env vars
## Multiple libraries
Cargo package (single directory) can't contain multiple libraries.

Prior art:
* [internals post (2015)](https://internals.rust-lang.org/t/how-about-changing-lib-to-lib-to-allow-multiple-library-in-a-crate/2022)
* [internals post (2018)](https://internals.rust-lang.org/t/multiple-libraries-in-a-cargo-project/8259)

Possible workaround: Create a workspace with multiple library 'packages'
## Dependencies per-crate target (instead of per-package)
Cargo can only express dependencies on a package level
## Can't easily specify RUSTFLAGS per crate target
Cargo [can specify](https://doc.rust-lang.org/cargo/reference/config.html#configuration-keys) additional `RUSTFLAGS` declaratively per *target-triple* in a local `.cargo/config`.
```toml
# For the following sections, $triple refers to any valid target triple, not the
# literal string "$triple", and it will apply whenever that target triple is
# being compiled to. 'cfg(...)' refers to the Rust-like `#[cfg]` syntax for
# conditional compilation.
[target.$triple]

#...

# custom flags to pass to all compiler invocations that target $triple
# this value overrides build.rustflags when both are present
rustflags = ["..", ".."]

[target.'cfg(...)']
# Similar for the $triple configuration, but using the `cfg` syntax.
# If several `cfg` and $triple targets are candidates, then the rustflags
# are concatenated. The `cfg` syntax only applies to rustflags, and not to
# linker.
rustflags = ["..", ".."]
```

## Can't specify linker args per-target
* [RFC #1766](https://github.com/rust-lang/rfcs/issues/1766)
* `link_args` in `rustc` - https://github.com/rust-lang/rust/issues/29596

# Other systems
## Bazel
How is it doing it? Rust support seems to be lacking (Rust code not officially approved for the Google monorepo?)
However Google Cloud uses Bazel - ppl should be using Rust there?
## GN
How Fuchsia folk integrate Rust?

[`rustc_library`](https://fuchsia.googlesource.com/build/+/master/rust/rustc_library.gni)
and
[`rustc_binary`](https://fuchsia.googlesource.com/build/+/master/rust/rustc_binary.gni)
rules (shared
[`rustc_artifact`](https://fuchsia.googlesource.com/build/+/master/rust/rustc_artifact.gni)
rules similar to Buck/Bazel)