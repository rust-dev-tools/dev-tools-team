# Roadmap - RLS and IDEs

See also [GH project](https://github.com/rust-lang-nursery/rls/projects/2)

## RLS

* Ride the trains to the stable channel
* beta, then 1.0 level of quality
* support common Cargo workflows (workspaces, lib/bin, cross-compilation)
* change model of operation to better integrate with incremental type checking/on demand compilation
* compiler-powered code completion
* auto-imports
* search (path search, text search, Rust-specific searches - e.g., find impls)
* power the new Rustdoc
* bettter testing
  - RLS tests on CI
  - more, better, easier RLS tests
  - test lower layers - save-analysis, rls-analysis
  - perf testing

## IDEs

* Improve RLS support outside VSCode
* Work out the VSCode plugin situation
* Make the plugin more complete (debugging, etc.)
* How to present stuff like borrow visualisation, macro debugging, etc.
