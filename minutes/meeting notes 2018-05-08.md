# May 8th, 2018

## Notes

### triage


PR: Stabilize suggestion applicability field in json output

* https://github.com/rust-lang/rust/pull/50486
* stabilises flag in JSON, not public API
* needed for Rustfix/edition stuff
* nrc to r+

PR: ship LLVM tools with the toolchain

* https://github.com/rust-lang/rust/pull/50336
* relatively small
* making a component would mean some more complexity in Rustup or build system (could use an empty component if tools are missing for a given toolchain)
* tooling (such as `cargo binutils`) could handle some of the complexity
* should we make lld a component? Maybe, but not as part of this PR
* Conclusion: lets make the tools a component, if possible


### teams and working groups

* IDE and editors
  - Need to find Racer maintainers, will investigate Racer nightly
* Rustdoc
  - Not much happening
* WG-bindgen
  - Not much happening
  - Can now inject Rust code into deep modules
  - Fuzzing happening soon
  - Still plenty to do in C++ land, but nothing pressing
* WG-Clippy
  - blog post on audit coming
* WG-doxidize
  - Not much happening
* WG-rustfmt
  - Working on the API for 1.0
* WG-rustup
  - Not much happening
* WG-edition (rustfix)
  - Rustfix is usable!
  - `cargo install --git ...`, (add edition attribute to Rust code), `cargo fix` (not many lints)
  - blocked on applicability flag, then lints PRs
  - open to contribution soon
  - 15th May preview: Rustfix - yes, Lints - maybe