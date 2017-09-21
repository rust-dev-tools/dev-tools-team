# These Weeks in Dev-Tools, issue 2

**2017-09-20**

Welcome to the 2nd issue of these weeks in dev-tools! We've had a bunch of tools
releases, some new people joining the team, and some accepted RFCs. We're also
just getting into the [impl period](https://blog.rust-lang.org/2017/09/18/impl-future-for-rust.html).
We're hoping the impl period can be a time where we really push forward on Rust
tools. There's [lots of interesting issues](https://www.rustaceans.org/findwork/impl),
so pick one, [chat to a mentor](https://gitter.im/rust-impl-period), and get
stuck in!

These Weeks in Dev-Tools will keep you up to date with all the exciting dev
tools news. We plan to have a new issue every few weeks. If you have any news
you'd like us to report, please comment on the [tracking issue](https://github.com/nrc/dev-tools-team/issues/28).

If you're interested in Rust's developer tools and want to contribute or ask
questions, come chat to us in #[rust-dev-tools](irc://moznet/rust-dev-tools).


## News

* We got [a website](https://www.rustaceans.org/findwork) for finding Rust issues
  to work on - lots of tools issues!
* @fitzgen and @matklad have become full members of the dev-tools team. @tromey
  has become a dev-tools peer for debuggers. [Announcement](https://internals.rust-lang.org/t/welcome-new-dev-tools-team-members/5948).
* Clippy and Rustfmt were added to [rust-lang/rust](https://github.com/rust-lang/rust/tree/master/src/tools)
  repo as submodules. This is the first step towards better stability by running
  their tests on Rust's CI and distribution with Rustup.
* The RLS is now available in beta. Install it with `rustup component add rls-preview`.
  It should hit stable with version 1.21. The `rls` component is being renamed
  to `rls-preview` on nightly too.
* [A Rust Docker image](https://hub.docker.com/_/rust/) was upstreamed by @sfackler.
* [rust-semverver](https://github.com/ibabushkin/rust-semverver) is a tool for
  automatically checking semver adherence.
* Rustdoc produces docs for the std libs for all platforms (including Windows!) - [#43348](https://github.com/rust-lang/rust/pull/43348)
* @booyaa gave a [talk on the RLS](https://skillsmatter.com/skillscasts/10664-rust-language-server-and-you)
* @Xanewok is giving a [talk about the RLS](http://zurich.rustfest.eu/sessions/igor) at Rustfest.
* Clippy has new lints: `infinite_iter`, `maybe_infinite_iter`, and `naive_bytecount`.
* Bindgen can time its phases (thanks to @jhod0!) - `--time-phases`.


## Releases

* [IntelliJ Rust](https://github.com/intellij-rust/intellij-rust) 0.1.0.2068: performance improvements, Cargo toolbar, [full change log](https://intellij-rust.github.io/2017/09/07/changelog-51.html)
* [Rustfmt](https://crates.io/crates/rustfmt-nightly) 0.2.7: new error format, yield support, more config options, preserve attributes on statements, [change log](https://github.com/rust-lang-nursery/rustfmt/blob/master/CHANGELOG.md)
* [bindgen](https://github.com/rust-lang-nursery/rust-bindgen) v0.30.0 - [announcement](https://users.rust-lang.org/t/bindgen-automatically-generate-rust-ffi-bindings-to-c-and-c-libraries/12126/2)
* [vscode Rust] 0.2.2: [changelog](https://github.com/rust-lang-nursery/rls-vscode/blob/master/CHANGELOG.md#022---2017-08-21) (and a new release any day now)


## RFCs

* [1615](https://github.com/rust-lang/rfcs/pull/1615) - Let Cargo [and other tools] put data into platform-specific directories - almost ready for FCP
* [1946](https://github.com/rust-lang/rfcs/pull/1946) - intra-rustdoc links - merged!
* [1990](https://github.com/rust-lang/rfcs/pull/1990) - add external doc attribute to rustc - merged!
* [2103](https://github.com/rust-lang/rfcs/pull/2103) - attributes for tools - merged!
* [2117](https://github.com/rust-lang/rfcs/pull/2117) - debuggable macro expansions - ready for experimental implementation


## Meetings

* Rustdoc rewrite [2017-08-14](https://github.com/nrc/dev-tools-team/blob/master/minutes/meeting%20notes%202017-08-14.md)
* RLS clients [2017-09-11](https://github.com/nrc/dev-tools-team/blob/master/minutes/meeting%20notes%202017-09-11.md)
