# Agendas and other info for the 2018 Rust all-hands in Berlin

## Goals

* Have a plan for tools and the 2018 edition
* Have plans for moving forward with Clippy, Xargo in Cargo, and Rustup in Cargo
* Be in agreement with other stakeholders around a solution for the bors queue/distro/tools problems
* Agreement with compiler team on the future of IDE support in the compiler
* Consensus on custom testing frameworks implementation
* Jel the new tools teams to some extent (not everyone will be present)

## Meeting slots

### Tools

* Tues 3:30 - 5:30
* Thurs 1:30 - 3:30
* Fri 10:00 - 12:00

* Compiler consumers (compiler/RLS/IDE integration): Mon 3:30 - 5:30
* What can infra do for you: Wed 1:30 - 2:30
* Website: Wed 2:30 - 3:30
* Libsyntax 2: Thurs 10:00 - 11:00

### Cargo

* Mon 3:30 - 5:30
* Fri 3:30 - 4:30

* Xargo integration: Wed 10:00 - 11:00
* Rustup integration: Wed 11:00 - 12:00

## Topics/planning

* Tuesday: 2018 edition planning
* Tuesday: Clippy lint audit (and work with compiler team on possibly moving lints in/out)
* Thursday: Compiler & tools (RLS and Clippy, overflow from compiler consumers' session)
* Thursday: Custom test frameworks
* Friday: IDE planning
* Friday: Tools overflow (TBD, decide during the week)

possibly covered in other sessions:

* Bors + infra, RLS, Clippy, other tools
* RLS/compiler integration (long term planning, perhaps covered in 'compiler consumers' session)
* Clippy/compiler APIs (perhaps covered in 'compiler consumers' session)

maybe side-sessions:

* RLS/save-analysis testing
  - test for stdout?
  - try running with -Zsave-analysis and lowering
  - ...
* Cargo refactoring and build system integration

## Agendas

### Cargo/Rustup integration

* scope
  - do we want all of Rustup? If not do we continue to support separate Rustup?
  - add stuff for discovering tools?
* UI plan
  - new Cargo commands?
* implementation plan
* risks?
* distribution story, IDE integration
* time estimate, resourcing

### Cargo/Xargo integration

https://paper.dropbox.com/doc/Xargo-integrationstd-aware-Cargo-KgXr5L75hAAYR0TxM4T3M

### Compiler and consumers

https://paper.dropbox.com/doc/Rust-All-Hands-2018-Compiler-and-its-consumers-zxdUwGHXzsGTs63zEEzg5

### 2018 edition planning

* tools to actively test
  - RLS, Rustfmt, Clippy, Bindgen, Rustdoc, Cargo
* encourage user testing
* formal testing plan?
* Rustfix
  - project plan: https://github.com/rust-lang/rust/issues/49247
  - who is responsible? killercup, manishearth
  - known issues?
  - QA
  - testing - users, CI

### Clippy lint audit

TODO (manishearth)

### Custom test frameworks

- merge https://github.com/rust-lang/rfcs/pull/2318
- discuss concrete first test_framework proc macro api
- test output
  - common api abstracting strcutured and human output
  - decide which cases to cover right now and which to leave open as "custom data"

### IDE planning

* Schedule for 1.0 and post-1.0 work
  - compiler code completion?
  - vscode extension 1.0 issues
* performance, stability, tech-debt, maintenance
  - long term and short term
  - compiler strategy
  - testing (save-analysis, RLS)
  - LSP changes
* brainstorm and prioritise feature work
  - do some detailed planning - do we want something like libsyntax2
* clients plan
* promotion/discovery
* intellij plans

### Compiler & tools

TODO (nrc) - TBD after 'compiler consumers' session

### Overflow session

TODO (nrc) - TBD during week


## Side-sessions

No room booked, no agenda, but the WG or team would like to chat in person

* RLS/save-analysis testing
  - test for stdout?
  - try running with -Zsave-analysis and lowering
  - ...
* Cargo refactoring and build system integration
* Rustup plans? (other than Cargo integration)
* bindgen planning?
* Doxidize planning

