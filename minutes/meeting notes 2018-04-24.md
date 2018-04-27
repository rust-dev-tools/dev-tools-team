# April 24th, 2018


## Notes

We did some triage and had an update from the sub-teams and working groups.

### triage

Stabilize the "approximate suggestion" flag, use for the epoch

  - https://github.com/rust-lang/rust/issues/50168
  - will use an enum, implement first then stabilisation FCP

RFC 1615 - Let Cargo put data into platform-specific directories

  - https://github.com/rust-lang/rfcs/pull/1615
  - ping Cargo team for thoughts. Do something (nrc).

### teams and working groups

  - Doxidize
    * merging back to rustdoc2
  - Clippy
    * Lint audit happened

        * link groups: https://rust-lang-nursery.github.io/rust-clippy/v0.0.194/index.html

        * 5 or 6 moved to nursery/pedantic
        * blog post or internals post to come
    * Talk to compiler team about lint exchange
    * approximate suggestions
  - RLS
    * 1.0 on track
  - Edition
    * add Rustfix to compiletest - tests that Rustfix makes a correct fix
    * missing from an end-to-end test: approximate or not suggestions. macros, notion of edition in Rustfix, revamp CLI
  - Rustfmt
    * Style RFC: next week
    * Stablity: What does it mean for rustfmt?
    * Proposed API: https://github.com/rust-lang-nursery/rustfmt/issues/2639
  - IDEs
    * Corrosion - Rust/LSP support in Eclipse https://github.com/eclipse/corrosion

