# 2018 edition planning 2018-03-27

* tools to actively test
  - RLS, Rustfmt, Clippy, Bindgen, Rustdoc, Cargo, rustup, IntelliJ
* encourage user testing
* formal testing plan?

* Rustfix
  - project plan
  - who is responsible?
  - known issues?
  - QA
  - testing - users, CI

## Notes

* new syntax happening
  - tools should work with it all by edition release
* tools releases not tied to edition
  - maybe RLS + rustfmt before, Clippy after
* edition indicated by entry in Cargo.toml, passed into rustc by Cargo
  - hopefully, RLS won't need to do anything
* unsure about the value of tracking/triaging
* use a single label across repos to identify edition issues
  - ACTION ITEM (nrc) Use GH API to collate
* encourage tool users to use the edition early
  - ACTION ITEM (all) do this
* ACTION ITEM (Manish) document Cargo config option for edition
* Tools could use the new edition in their test suites
* Rustdoc
  - adding flag for compiling and building doc tests with new edition
  - set flag for rustdoc tests? Do we want to run with both editions?
    * general question what are we doing with compiler tests, etc.?
    * ACTION ITEM (misdreavus) find out what compiler team are doing with testing
  - churn in display of new features
    * ACTION ITEM (rustdoc team) May - audit these layouts
  - `?` in doc tests - follow unit tests?
* Cargo
  - field exists
    * passes a flag to rustc (currently broken)
    * TODO pass to rustdoc too
  - add a command line option
  - perhaps remove some deprecated stuff? Decide soon!
* Rustfmt
  - ACTION ITEM May (Rustfest is 27th) - ensure we're formatting new language features

* Rustfix
  - tracking issue: https://github.com/rust-lang/rust/issues/49247
  - ACTION ITEM (Manish) - tracking issue for edition lints
  - UI: `cargo fix` will just fix everything
  - some finer-grained control available
  - beta release end of April
  - then rustup dist
  - won't be complete - macros - need to check how big an issue that is
  - mgattozzi, killercup, oli-obk, Manishearth
  - RLS command/VSCode task to run rustfix?
