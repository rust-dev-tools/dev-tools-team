# Rust tools

## Building Rust programs

### rustc - the compiler

You'll probably never use the compiler directly; the usual way to build a Rust
program is to use Cargo. Part of the Rust distribution.

* [Source code](https://github.com/rust-lang/rust).
* [Contributing](https://github.com/rust-lang/rust/blob/master/CONTRIBUTING.md)
* [Dev docs](https://forge.rust-lang.org/)
* Install using Rustup (see below).

### Cargo

Cargo is Rust's package manager and build system. It is distributed with Rust by
default so you don't need to use it.

* [Source code](https://github.com/rust-lang/cargo)
* [Documentation](http://doc.crates.io/)
* Install using Rustup (see below).

### Rustup

Rustup is a tool for installing Rust and managing Rust toolchains, for example
for different channels (nightly, beta, etc.) or target platforms for cross-
compilation.

* [Install](https://www.rustup.rs/)
* [Source code](https://github.com/rust-lang-nursery/rustup.rs)
* [Documentation](https://github.com/rust-lang-nursery/rustup.rs/blob/master/README.md)

### Xargo

Xargo is a tool to help with cross-compilation. It builds and manages versions
of the standard libraries, and manages Cargo targets. It is a drop-in
replacement for Cargo, e.g., you use `xargo build` instead of `cargo build`.

* Install using Cargo: `cargo install xargo`, or download a [binary release](https://github.com/japaric/xargo/releases).
* [Source code](https://github.com/japaric/xargo)
* [Documentation](https://github.com/japaric/xargo/blob/master/README.md)

### cargo watch

A tool that watches the disk for changes and starts a build when you change a
file in your project. Run with `cargo watch`.

* Install with `cargo install cargo-watch`.
* [Source code](https://github.com/passcod/cargo-watch)
* [Documentation](https://github.com/passcod/cargo-watch/blob/master/README.md)
* [Developer docs](https://github.com/passcod/cargo-watch/blob/master/CONTRIBUTING.md)


## IDEs and editors

TODO

* intellij
* vs code
* RLS
* sublime, atom, emacs, vim
   - syntax highlighting
* Racer

## Debugging and profiling

TODO

* gdb - versions, script?
* lldb
* vs

## Formatting

Rustfmt is a tool for automatically formatting Rust code. It enforces the
officially recommended Rust style, or can be configured to your taste. It is
used by the RLS, so if you're using an RLS-powered IDE, you don't separetly need
Rustfmt. If you're not, you can use it with Cargo (`cargo fmt`) or standalone
(`rustfmt`).

* Install using `cargo install rustfmt-nightly` (if you use nightly Rust) or
  `cargo install rustfmt` (if you use stable Rust).
* [Source code](https://github.com/rust-lang-nursery/rustfmt)
* [Documentation](https://github.com/rust-lang-nursery/rustfmt/blob/master/README.md)
* [Developer docs](https://github.com/rust-lang-nursery/rustfmt/blob/master/Contributing.md)

## Working with other languages

TODO

* bindgen - https://servo.github.io/rust-bindgen/
* not tools - neon, helix, etc.

## Linting

### clippy

Clippy is a collection of lints to catch common mistakes and improve your Rust code.
It also covers code style issues that aren't just syntactical and require type information.
It comes as a compiler plugin and as a cargo subcommand (`cargo clippy`).

* Install using `cargo install clippy` (requries nightly)
* [Source code](https://github.com/Manishearth/rust-clippy)
* [Lint Documentation](https://github.com/Manishearth/rust-clippy/wiki)
* [Contribution docs](https://github.com/Manishearth/rust-clippy/blob/master/CONTRIBUTING.md)

TODO

* sanitisers

## Documentation and code exploration

TODO

* rustdoc
* rustdoc 2
* rustw
* docs.rs

## Testing, etc.

TODO

* test, bench
* cargo fuzz
* compiletest
* coverage
