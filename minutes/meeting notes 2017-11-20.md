# November 20th, 2017

Agenda week, Vidyo meeting: 2018 planning

## Notes

The meeting covered initial planning for dev tools in 2018. This was basically
'requirements gathering' and brain storming, with no attempt at prioritisation
yet.


### Some overall thoughts

* Lots of ongoing work from this year
* Likely that consolidation will be a big part of next year's work
* Debugging is a big area for work.
* Moving Rustup functionality into Cargo will be very user-facing
* Continue to improve workflow around tools distribution (and integration with Rust repo).
* We should improve tool ergonomics across the board
  - in particular being on nightly sucks
* Could we abstract over the compiler a bit more to have a uniform API for tools?


### WASM

* Possibly big next year
* What will debugging look like
  - Can we ensure Rust will be well-supported (c.f., DWARF)
* Build tools, webpack integration (already started)


### IDEs

* Refactoring
* Debugging

RLS:

* 1.0
* compiler-driven code completion
* stability (users and clients need it)
* better Clippy integration (Rustfix approach? - killercup)

IntelliJ:

* macro support
* feature work - refactoring, etc.
* debugging - blocked on CLion (need to make it work with Cargo)


### Debugging

Goal: less pain!

* LLDB support
* better Dwarf info from compiler in parallel with debuggers
* expression parsing - can we recycle bits of the compiler? Lots of trade-offs!


### Rustdoc

New rustdoc:

* exist!
* on a par with current rustdoc
  - good enough to stop working on current rustdoc, not necessarily bug-for-bug compatible
* experiment with stuff (book views, source exploration, etc)

Current rustdoc:

* Continue incremental improvements
* paths/links RFC (compiler work)


### Clippy

* rustup distribution
* more lints
* factor util code into rustc? (Abstraction layer?)
* 1.0 release
* improve suggestions (auto-application, rustfix)


### Testing

* extensibility is important
* also nice to have better default system - setup/teardown, etc.
* can we make it extensible enough to experiment with?
* inspecting output in IDEs
* JSON output
* fuzzing
* doc tests too want setup/teardown


### Cargo and Xargo

* support local registries

Xargo:

* make it part of Cargo and disappear!
* coordinate with rustup into Cargo stuff
  
