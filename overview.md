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

Many editors have some Rust support, from basic syntax highlighting to full IDE features.

For IDE support we recommend either VSCode with the Rust Language Server or
IntelliJ. The Rust Language Server is an editor-independent service for
providing IDE functionality using the [Language Server Protocol](http://langserver.org/).
Any editor can be an RLS client and we hope many editors will take advantage of it.

### Visual Studio Code

[Visual Studio Code](https://code.visualstudio.com/) is a cross-platform, open
source IDE from Microsoft. It has very basic support (syntax highlighting) out
of the box. For better Rust support, install the Rust (RLS) plugin:

* [In the VSCode Marketplace](https://marketplace.visualstudio.com/items?itemName=rust-lang.rust)
* Install in VSCode with `ext install rust` in the command palette.
* [Source code](https://github.com/rust-lang-nursery/rls-vscode)
* [Developer docs](https://github.com/rust-lang-nursery/rls-vscode/blob/master/contributing.md)

### The RLS

The power behind the above VSCode extension is the RLS. It interacts with the
compiler to provide information about Rust programs to IDEs and editors. You
probably don't want to use it directly, but rather via an editor extension.

* Install with `rustup component add rls` (but your editor should do this for you)
* [Source code](https://github.com/rust-lang-nursery/rls)
* [Documentation](https://github.com/rust-lang-nursery/rls/blob/master/README.md)
  and [developer docs](https://github.com/rust-lang-nursery/rls/blob/master/contributing.md)

### IntelliJ

Rust support in IntelliJ is provided by JetBrains via an open source plugin,
written in [Kotlin](https://kotlinlang.org/). It uses its own Rust programming
language model instead of RLS, so some features available in RLS might not be
available in IntelliJ and vice verse.

* [Website](https://intellij-rust.github.io/)
* [Source code](https://github.com/intellij-rust/intellij-rust)
* [Documentation](https://intellij-rust.github.io/docs/quick-start.html),
  [developer docs](https://github.com/intellij-rust/intellij-rust/blob/master/CONTRIBUTING.md),
  and [architecture docs](https://github.com/intellij-rust/intellij-rust/blob/master/ARCHITECTURE.md).

### Other editors

Many editors have plugins or extensions that provide some degree of support
(syntax highlighting, snippets, etc.). **Racer** is a tool for providing code
completion and 'goto def' features. It uses it's own parsing and type inference,
so is very quick, but sometimes incomplete. There are extensions using Racer for
many editors.

* [Racer source](https://github.com/racer-rust/racer)
* [Racer docs](https://github.com/racer-rust/racer/blob/master/README.md)
* There is [editor support for Racer](https://github.com/racer-rust/racer/blob/master/README.md#editorsides-supported)
  in Atom, Eclipse, Emacs, GEdit, Kakoune, Kate, Sublime Text, Vim, and Visual
  Studio Code.
* The Rust teams maintain editor support (configuration and/or syntax highlighting)
  for [Sublime Text](https://github.com/rust-lang/sublime-rust),
  [Emacs](https://github.com/rust-lang/rust-mode), [Vim](https://github.com/rust-lang/rust.vim),
  [GEdit](https://github.com/rust-lang/gedit-config), [Nano](https://github.com/rust-lang/nano-config),
  and [Kate](https://github.com/rust-lang/kate-config).


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

### Clippy

Clippy is a collection of lints to catch common mistakes and improve your Rust code.
It also covers code style issues that aren't just syntactical and require type information.
It comes as a compiler plugin and as a cargo subcommand (`cargo clippy`).

* Install using `cargo install clippy` (requries nightly)
* [Source code](https://github.com/rust-lang-nursery/rust-clippy)
* [Lint Documentation](https://github.com/rust-lang-nursery/rust-clippy/wiki)
* [Contribution docs](https://github.com/rust-lang-nursery/rust-clippy/blob/master/CONTRIBUTING.md)

TODO

* sanitisers

## Documentation and code exploration

### Rustdoc

Rustdoc generates API documentation for Rust crates.  You probably don't need to use rustdoc
directly (instead going through `cargo doc`). Accepts a crate and generates HTML pages for its
public documentation. Can also execute code blocks within the documentation as tests. Part of the
normal Rust distribution.

* [Source code](https://github.com/rust-lang/rust/tree/master/src/librustdoc) (part of the Rust
  compiler source code)
* Install using Rustup (see above). Installed by default when you get rustc.

### Rustdoc 2

In-progress reimplementation of rustdoc using the RLS as a backend (rather than compiler-internal
libraries). Plans include JSON output, multiple pluggable "frontends" to render as different
formats, and separating out the HTML design to allow more contributors to work on the site design.

The initial plans for the new rustdoc were discussed [during the dev-tools meeting on
2017-05-30][2017-05-30].

[2017-05-30]: https://github.com/nrc/dev-tools-team/blob/master/minutes/meeting%20notes%202017-05-30.md

* [Source code](https://github.com/steveklabnik/rustdoc)
* [Contribution docs](https://github.com/steveklabnik/rustdoc/blob/master/CONTRIBUTING.md)

TODO

* rustw
* docs.rs

## Testing, etc.

TODO

* test, bench
* cargo fuzz
* compiletest
* coverage
