# June 12th 2017

Focussed week - contribution, Vidyo meeting

## Notes

```
mk - architectural docs
   - source code organisation
mw - minimise API needed to know to contribute, e.g., Clippy
kc - small tools (and small components) less intimidating (tools inside rustc repo are hard to build)
jt - compiler error messages were successful - big project, but clear instructions, list of stuff to tick off
ba - +1 structure helps, small, discreet steps
mg - too many layers of abstraction vs componenents (e.g., Cargo vs Clippy)
mw - tests for incr compilation, similar setup to errors - worked, but didn't convert many contributors to serious contributors
ba - also lib blitz
nf - ladder of more interesting/challenging issues
jt - encourage to take initiative - be responsive mentors
   - high impact (roadmap)
me - suggest a bug, or ping people when something similar comes up, guide into more general stuff,
kc - also ask for review, opinion
ba - high cost mentoring, prefers processes/procedures
me - get contributors mentoring each other, spreadsheet to track contributors (uses BigQuery over GH)
ba - can we centralise coordination for dev tools?
mw +1
ba - can we automate with GH?
jt - look for proactive people
nf - GH issue - comment here if you want something to do - could do it for all of dev tools
nc - how to get more paid people contributing
matklad - get people to fix bugs they find
  - not actively asking people, but if people report easy issues, then give clear instructions to fix
motivation - FAME!, fixing bugs, interesting stuff, productivity benefits for lots of people, scratching an itch
jt/nrc - users leading to contributing - "all users are contributors" in OSS
kc - add to help message - GH link (like compiler - been successful there)
ba - Rust tooling story - need to tell it better, advertise status, ongoing, announcing releases
sk - general discoverability
jt - specific issues as well as roadmap
nf - review each others contribution docs
   - checklist for being a good tool (similar to lib blitz)
nc - unify processes
mg - servo starters - collect starter bugs
nc - +1
kc,nc - how to keep people involved
nc - acknowledgement
mw - keep it interesting, remove frustrations, technical debt
jt - beware of too clever code
mw - clear extension points for tools - good for contribution, good for tools
kc - clever code is OK if it is hidden/abstracted away, problem if it is everywhere
   - need to encourage people who want challenges - document clever code
nf - non-technical side? Why contribute? Path to maintaining, other recognition
jt - acknowledge people on release notes
kc - can we have a process for all tools for path to maintainer, etc.?
mg - wary of defined path, leads to gaming, e.g., wikipedia, issues with interpreting the rules
jt - guidelines rather than rules
nf - highlight that it is possible to progress
```



### killercup's notes

* to many people, small, isolated tools feel easier to contribute to than rust-lang/rust (or even cargo)
  - many tools currently depend on being in-tree, or at least being built in-tree, but integrating clippy/rls shows it doesn't have to be this way
* have a 'contributing' guide in each project, describing how to get set up, and who/where to ask (probably irc for most things)
* increase visibility of internal tools/libs
  - e.g. a blog post describing how cargo's test infrastructure works +1
* low hanging fruit: let people know we want their help
  - e.g.: we have a lot of cli tools. their help messages could contain something like "Something missing? Let us know/help us out: <github.com/foo/bar>"
  - same for panics


### Links

* https://whatcanidoformozilla.org/
* https://www.joshmatthews.net/bugsahoy/
* https://starters.servo.org/
* https://www.joshmatthews.net/oscon16/


### nrc's thoughts/experience

* contribution funnel: experimenters > users > casual contributors > dedicated contributors
  - need to increase numbers at the start, and make it easy to make each step
* improve discoverability
  - visibility
* remove barriers to entry
  - docs
    - including GH/PR process
  - setup env, build
  - finding things to work on
* remove barriers to progression
* motivation (real and perceived)
  - fun
  - learning
  - useful
  - impact
  - reputation
* momentum
* how do we keep maintainers? (I don't feel I've ever really done this well)
  - killercup: this-week-in-rust could feature long-term contributors to important tools
  - killercup: gamification? (e.g. a dashboard with streaks and featured maintainers)
  - killercup: consider especially asking paid employees of companies using rust, they tend to have more time (sad but true)
* rough edges to fix

### some concrete ideas

* visibility
  - tools blog post
  - extend, make stdx-dev more useful?
* tools contribution start page
  - describe each tool, list some good starting bugs, mentor info
  - link from r-l.o, twir
* announce releases
* review each others CONTRIBUTING.md
* Use the same set of tags for issues across tools ("easy" vs "E-Easy" etc)

