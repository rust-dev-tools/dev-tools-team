# May 22nd, 2018

## Notes

### triage

* RFC: rustfmt stability
  - https://github.com/rust-lang/rfcs/pull/2437
  - important for users running Rustfmt in CI, input from team would be good
  - Perhaps relevant for Clippy too


## Clippy 1.0

* only major blocker is lint audit visibility - blog post
  - will add text on stability
* Clippy team to track breaking changes so we have some data
* stability questions - rustfmt style or lint groups, etc. Clippy team have already discussed, conclusions will be added to lint audit blog post.


### stabilising scoped attrs

* e.g., `rustfmt::skip`
* used by Clippy and Rustfmt
* blocks Rustfmt 1.0 (and probably Clippy)
* not been implemented long, but I'd like to stabilise soon-ish


### Discord?

* some energy to move from Gitter to Discord for Rust comms
* existing discord already has a dev-tools channel
* generally in favour
* `@` icon gives a list of notifications


### New error emission crate

* useful for rustc and tools
* https://github.com/zbraniecki/annotate-snippets-rs


### teams and working groups

* rustfmt
  - new API and CLI
* RLS
  - good crash reports really useful
    * Just opening Diesel repo
* Rustdoc/Doxidize
  - Doxidize on pause till after edition
  - Focus on text of docs
* Rustfix
  - `cargo fix` CLI - from tomorrow(?) `cargo install cargo-fix`
* Clippy
  - suggestion applicability in progress
* Racer
  - has new maintainers \o/

