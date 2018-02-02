# These Weeks in Dev-Tools, issue 3

**2018-02-02**

Welcome to the 3rd issue of these weeks in dev-tools! It's been a while since
the last issue, sorry. To make up for it, there is a lot this time around.

These Weeks in Dev-Tools will keep you up to date with all the exciting dev
tools news. We plan to have a new issue every few weeks. If you have any news
you'd like us to report, please comment on the [tracking issue](https://github.com/nrc/dev-tools-team/issues/35).

If you're interested in Rust's developer tools and want to contribute or ask
questions, come chat to us in #[rust-dev-tools](irc://moznet/rust-dev-tools).


## News

* [how does the RLS work?](https://www.ncameron.org/blog/how-the-rls-works/).
* [when will the RLS be released](https://ncameron.org/blog/when-will-the-rls-be-released/).
* New Rustdoc is progressing fast, now supporting [almost all DefKinds](https://github.com/steveklabnik/rustdoc#177), [pluggable frontends](https://github.com/steveklabnik/rustdoc#191), [More powerful testing](https://github.com/steveklabnik/rustdoc#203).
* Trait objects can now be inspected in the debugger (patches in rustc, llvm, gdb).
* Rust support in [SearchFox](https://github.com/mozsearch/mozsearch/pull/50).
* Rustfmt options are getting ready for 1.0, see [configurations.md](https://github.com/rust-lang-nursery/rustfmt/blob/master/Configurations.md).
* [Rust support in Visual Studio](https://marketplace.visualstudio.com/items?itemName=DanielGriffen.Rust) using the RLS (VS, not VS Code).
* Announcing [Rerast](https://github.com/google/rerast), a tool for automatically rewriting Rust code.
* [Semverver](https://github.com/rust-lang-nursery/rust-semverver), a tool for checking semver compatibility, has moved to the nursery.
* [Rust Enhanced hits 50k downloads](https://jason-williams.co.uk/rust-enhanced-reaches-50k-downloads/) (Sublime Text plugin).

## Releases

* [IntelliJ Rust](https://github.com/intellij-rust/intellij-rust) 0.2.0, [announcement](https://users.rust-lang.org/t/intellij-rust-0-2-0-released/13419)
* [Rustfmt](https://crates.io/crates/rustfmt-nightly) 0.3.7, [change log](https://github.com/rust-lang-nursery/rustfmt/blob/master/CHANGELOG.md)
* [bindgen](https://github.com/rust-lang-nursery/rust-bindgen) v0.31.0 - [announcement](https://users.rust-lang.org/t/bindgen-automatically-generate-rust-ffi-bindings-to-c-and-c-libraries/12126)
* [vscode Rust](https://marketplace.visualstudio.com/items?itemName=rust-lang.rust) 0.2.3: [changelog](https://github.com/rust-lang-nursery/rls-vscode/blob/master/CHANGELOG.md#023---2017-09-21)
* [Atom IDE](https://github.com/mehcode/atom-ide-rust#install) v0.11 - [tag](https://github.com/mehcode/atom-ide-rust/releases/tag/v0.11.0)


## RFCs

* [2285](https://github.com/rust-lang/rfcs/pull/2285) - Update the disambiguation handling in RFC 1946 (intra-rustdoc-links) to match impl concerns - FCP merge
* [2287](https://github.com/rust-lang/rfcs/pull/2287) - Benchmarking / cargo bench  - FCP close - superseded by 2318
* [2318](https://github.com/rust-lang/rfcs/pull/2318) - Post Build Contexts (custom test frameworks)


## Meetings

Find all meeting notes [here](https://github.com/nrc/dev-tools-team/tree/master/minutes), some highlights:

* Cargo build system integration [2017-11-06](https://github.com/nrc/dev-tools-team/blob/master/minutes/meeting%20notes%202017-11-06.md)
* 2018 planning [2017-11-20](https://github.com/nrc/dev-tools-team/blob/master/minutes/meeting%20notes%202017-11-20.md)
* Testing and custom test frameworks 1 [2018-01-11](https://github.com/nrc/dev-tools-team/blob/master/minutes/meeting%20notes%202018-01-11.md)
* Testing and custom test frameworks 2 [2018-01-23](https://github.com/nrc/dev-tools-team/blob/master/minutes/meeting%20notes%202018-01-23.md)
