# Clippy lint audit planning

* 3 lint groups
  - clippy (deny/warn by default; 200)
    * correctness
    * performance heuristics
    * style (`if let` instead of `match`)
  - clippy pedantic (allow by default - false positives or disagreement; 40)
  - restriction lints (e.g., `= .. + .. ` over `+=`, debatable style issues; 10)
* current expectation is to disable many lints
* proposal: add lint groups (e.g, correctness, see above)
* proposal: err on the side of making lints pedantic
* proposal move style lints from compiler to Clippy and some correctness lints from Clippy to compiler
  - get compiler team input
  - will need to add a `moved` state to the compiler (c.f., `removed`)
* on moving *all* lints to Clippy: there is benefit to having the opt-in of running Clippy
  - basically, no
* proposal add nursery (unstable) category of lints
* do we use scoped lints?
  - `#[clippy::foo]` or `#[foo]`
  - moving from compiler to clippy would take effort
* documentation for all lints?
  - specification?
  - documentation exists, but not discoverable (compiler)
  - might require more in some places
  - TODO improve documentation of compiler lints and make it discoverable (tooling)


## Plan

* Output: Clippy PR
* starting NOW
  - initial audit ~ 1 month
  - RFC (or whatever) done by September
* For each clippy lint (clippy team): assign to a group (correctness, performance, style, pedantic, restriction, nursery) and (deny/warn/allow)
* triage clippy -> compiler lints (compiler team): correctness lints only
* For each compiler lint (compiler team): should it move to Clippy (or rustfmt); if so triage as above
  - some built-in lints can't be moved to Clippy because they happen within a phase
* RFC?
  - worries about bikeshedding
  - maybe do it in it's own repo
  - adds legitimacy
  - debate and implement first, then 1.0
  - ACTION ITEM (nrc) get core team thoughts
  - ACTION ITEM (clippy team) decide what to do

## Status

* lint audit done: https://github.com/rust-lang-nursery/rust-clippy/pull/2579
