# These Weeks in Dev-Tools, issue 1

**2017-08-TODO**

Welcome to the first ever issue of 'These Weeks in Dev-Tools'! The dev-tools
team is responsible for developer tools for Rust developers. That means any
tools a developer might use (or want to use) when reading, writing, or debugging
Rust code, such as Rustdoc, IDEs, editors, Racer, bindgen, Clippy, Rustfmt, etc.

These Weeks in Dev-Tools will keep you up to date with all the exciting news
in this area. We plan to have a new issue every few weeks. If you have any news
you'd like us to report, please comment on the [tracking issue](https://github.com/nrc/dev-tools-team/issues/25).


* Announced and released the [RLS Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=rust-lang.rust)
* [Clippy](https://github.com/rust-lang-nursery/rust-clippy) and [Bindgen](https://github.com/rust-lang-nursery/rust-bindgen) moved into the [rust-lang nursery]()
* [Rust IntelliJ support is official!](https://blog.jetbrains.com/blog/2017/08/04/official-support-for-open-source-rust-plugin-for-intellij-idea-clion-and-other-jetbrains-ides/)
* Blog post - [one environment to rule them all](https://xanewok.github.io/gsoc/2017/one-environment-to-rule-them-all/) - how the RLS handle environment variables by Xanewok
  - Xanewok also has a few other [blog posts](https://xanewok.github.io/gsoc/) about his GSoC project working on the RLS
* Blog post - [what the RLS can do](http://www.ncameron.org/blog/what-the-rls-can-do/) by nrc


## Releases

* [Rustfmt](https://crates.io/crates/rustfmt-nightly) 0.2 and 0.2.1
  - No longer saves backups by default
  - Orders imports and `extern crate`s
* [RLS Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=rust-lang.rust)
  0.1, 0.2.0, 0.2.1: [changelog](https://github.com/rust-lang-nursery/rls-vscode/blob/master/CHANGELOG.md)
* [Racer](https://github.com/racer-rust/racer) 2.0.10 [changelog](https://github.com/racer-rust/racer/blob/master/CHANGELOG.md#2010)
* [Bindgen](https://github.com/rust-lang-nursery/rust-bindgen) 0.29.0 [announcement](https://users.rust-lang.org/t/bindgen-automatically-generate-rust-ffi-bindings-to-c-and-c-libraries/12126)
* [IntelliJ Rust](https://github.com/intellij-rust/intellij-rust) #48 [changelog](https://intellij-rust.github.io/2017/08/07/changelog-48.html)


## RFCs

* [Add external doc attribute to rustc](https://github.com/rust-lang/rfcs/pull/1990) has been proposed to enter FCP


## Thanks!

* [@photoszzt](https://github.com/photoszzt) has been re-writing various ad-hoc computations into fix-point analyses in Bindgen:
  - whether we can add `derive(Debug)` to a struct: rust-lang-nursery/rust-bindgen#824
  - and whether a struct has a virtual table: rust-lang-nursery/rust-bindgen#850
* @topecongiro for doing sustained, impressive work on Rustfmt - implementing
  the new RFC style, fixing (literally) hundreds of bugs, and lots more.
* Shout out to @TedDriggs for continuing to push Racer forward. Jwilm and the
  rest of Racer's users continue to appreciate all your hard work!


## Meetings

We've had a bunch of meetings. You can find all the minutes [here](https://github.com/nrc/dev-tools-team/tree/master/minutes).
Some that might be interesting:

* dev-tools team roadmap [2017-07-31](https://github.com/nrc/dev-tools-team/blob/master/minutes/meeting%20notes%202017-07-31.md)
* Xargo and cross-compilation [2017-06-26](https://github.com/nrc/dev-tools-team/blob/master/minutes/meeting%20notes%202017-06-26.md)

