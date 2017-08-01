# July 31st 2017

Agenda week, roadmap


## Background

* Issue: https://github.com/nrc/dev-tools-team/issues/1
* Existing documentation: https://github.com/nrc/dev-tools-team/tree/master/roadmaps/README.md

## Notes

### motivation

* build awesome tools
  - make tooling a reason to use Rust
* make tools people like and talk about
* what tools do we need? where should we be?
  - established tools c.f., new tools
* find the new tools which need building

### What we're planning for

* this year
  - impl quarter - https://internals.rust-lang.org/t/announcing-the-impl-period-sep-18-dec-17/5676
  - work to be ready for contributors
* next year - intiail planning

### go over goals

#### Improve contribution

* deadline - be ready for the impl quarter
* hard to hang on to contributors
* we should track mentoring
* servo starters for tools
  - focus on ease of contribution for tools
* don't lose momentum with contributors! Help get stuck PRs unstuck, etc.

#### Discovery and delivery

* progress on better promotion of tools
* goal of moving some tools to rustup distribution
* better integration (e.g., installing missing tools) not likely until next year
  - can we patch into rustup?
* c.f., Go - don't need to install anything extra

#### IDE support

docs for LSP/RLS integration would be good to have

#### Rustdoc

* focus on rustdoc2
  - progress - module system + docs
* https://gist.github.com/QuietMisdreavus/3c0702517bea40711ed9baf513971143

#### Better understand what devs want/what is missing

Ongoing...

#### Explorative: tools for learning

No progress.

#### Explorative: integration with other languages

bindgen - bug fixing, polish, etc. motivated by Stylo work. Contribution work (good for other projects too). Probably a ways off 1.0


#### what are we missing?

* 1.0
  - several tools getting close to 1.0 this year - Clippy, Rustfmt, RLS
  - but not Bindgen
* debugging - miri
  - big thing for next year, but resourcing problem (might have help here)
  - at least need to advertise the state of the art
* Cargo
  - Should we be working with the Cargo team?
  - cargo edit
  - cargo watch


### Not covered

* plans for impl block?
  - and do we need to get anything done before then?
* thoughts on 2018?
* evolving the team?
* individual tools?
