# Rust dev-tools team

This repo contains resources for the Rust dev-tools team.

The dev-tools team is responsible for developer tools for Rust developers. Dev
tools means any tool a Rust programmer might use when writing, reading, testing,
or running Rust programs. For example, Rust integration with editors or IDEs,
Rustdoc, bindgen, etc.

Dev-tools is an umbrella team and covers several teams and working groups
dedicated to specific tools. See the [org chart](org-chart.md) for details.

Read the [latest dev-tools news](https://github.com/nrc/dev-tools-team/blob/master/twidt/issue-3.md).

For news as it happens, see the [current tools news issue](https://github.com/nrc/dev-tools-team/issues/35).

## Links to tools

* [Rustup](https://github.com/rust-lang-nursery/rustup.rs)
* [The RLS](https://github.com/rust-lang-nursery/rls)
  - [rls-vscode](https://github.com/rust-lang-nursery/rls-vscode)
  - [rls-analysis](https://github.com/nrc/rls-analysis)
  - [rls-vfs](https://github.com/nrc/rls-vfs)
* [Rustfmt](https://github.com/rust-lang-nursery/rustfmt)
* [Rustdoc](https://github.com/rust-lang/rust/tree/master/src/librustdoc)
* [Clippy](https://github.com/rust-lang-nursery/rust-clippy)
* [bindgen](https://github.com/rust-lang-nursery/rust-bindgen)
* [IntelliJ Rust](https://github.com/intellij-rust/intellij-rust)
* ... (TODO)

## 2018 Roadmap

* ship it!
  - rustfmt 1.0
  - RLS 1.0
  - VSCode 1.0
  - Clippy 1.0
* epoch transition
  - existing tools should cope
  - new tools as required
* a tools presence on the website
* cargo - TBA
* test frameworks
  - RFC
  - unstable implementation
  - re-implement #[test] and #[bench] as custom test frameworks
  - supporting wasm and embedded test frameworks
* doxidize (rustdoc 2)
  - initial release
  - usable for real projects
* maintain and improve
  - rustdoc, rustup, bindgen, debugging, editors
