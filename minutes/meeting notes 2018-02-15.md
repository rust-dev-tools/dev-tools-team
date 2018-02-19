# February 15th, 2018

Agenda week, Vidyo meeting: 2018 Roadmap and team organisation

## Notes

We discussed the roadmap initially at the end of last year. At this meeting we
went over a list of goals and agreed they were good for the team.


### Roadmap

* ship things!
  - rustfmt 1.0 and rustup dist
  - RLS 1.0
  - VSCode 1.0
  - Clippy 1.0
    * 1.0 but carefully define what stability means
    * look into distribution (rustup, alternatives?)
    * audit lints - categorise, make buggy ones allow by default
    * move towards a more stable API
* epoch transition
  - existing tools should cope
    * keep in mind for Clippy lint audit
    * bindgen target version?
  - new tools (for automatically transitioning code from old epoch to new)
* website (get tools info on the website)
* cargo (not really covered in the meetings)
* test frameworks
  - RFC
  - rustc implementation - easy
  - Cargo integration harder, but do-able this year
  - re-implement #[test] and #[bench] as custom test frameworks
  - supporting wasm and embedded test frameworks
* doxidize (rustdoc 2)
  - initial release
  - usable for real projects
* maintain and improve
  - rustdoc, rustup, bindgen, debugging, editors
* Domain Working Groups (servers, CLI, embedded, WASM; as defined in the roadmap) are starting up soon
  - Cross-cutting concerns: domain working groups will interact with/inform regular Rust teams


### Reorganising the team

* sub-teams
  - working group vs sub-team
    * WG is goal-focused and possibly temporary.
    * Sub-team has decision making power, meetings, leader
  - irc/gitter channels, point of contact for contributors
* scaling - more teams mean we can bring on more people
* no more peers - it has served well, but doesn't scale well
* pull in Cargo team
* one goal: fewer meetings for most people

* sub-teams
  - IDEs and editors (including Racer and RLS)
  - rustdoc
  - Cargo (including Xargo, etc)
* WGs
  - Clippy (should transition to sub-team in near future)
  - testing (custom test frameworks)
  - doxidize (should merge with rustdoc team, eventually)
  - rustfmt
  - rustup (should merge with Cargo sub-team)
  - bindgen
  - debugging
  - in the future: program transformation (rustfix, rerast, refactoring)/epoch transition

### overhaul find-work site

We should do this soon
