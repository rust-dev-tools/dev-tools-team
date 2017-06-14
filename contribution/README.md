# Contribution

We want to make tools an attractive domain for people to contribute to the Rust
community. For that we need to attract more contributors and make life better
for them.

We discussed this at the [2017-06-12 meeting](../minutes/meeting%20notes%202017-06-12.md).

## Removing barriers to entry

* A [checklist](checklist.md) for each tool/repo to follow
* Improved documentaion
  - architectural overview
  - source code organisation
* get others on the team to review


## Attracting contributors

* Initiatives have been successful, e.g., compiler error messages, libs blitz
  - describe in detail what needs to be done for each case (small, discrete steps)
  - make a checklist of work todo
  - advertise with motivation, find the impact
* good-first-issue/e-easy issues
* encourage people to fix issues they report by giving instructions to fix
* 'comment here if you want to contribute' issue
* add links to error messages


## Mentoring

* provide a ladder of more interesting/challenging issues to work
* name mentors to contact (and how to ping)
* make clear that there is a path to seniority
* encourage contributors to mentor as part of their growth
* after first issue, suggest similar issues and/or ping when related issues appear
* ask contributors for opinions and reviews
* look for proactive people
* acknowledge contribution


## Team-wide ideas

* Improve visibility of tools in general
  - see [issue 9 - discoverability](https://github.com/nrc/dev-tools-team/issues/9)
  - tell the tools story
  - needs to be ongoing - releases, status reports - low-level (specific issues) as well as high-level (roadmap), acknowledge contribution
  - blog posts on tools - how they work, etc.
* central tracking of good first issues (see https://starters.servo.org/)
* central documentation of how-to-contribute resources
  - in particular how to progress as a contributor
* track contributors to ensure good experience
  - automated integration with GitHub
* unify processes between tools (can we use the same tags, etc.)


## Code and design

* Design with extension points in mind
* Minimise the API required to contribute
* Smaller code bases are less intimidating
* Avoid too many layers of abstraction (or complexity in general)
  - where 'clever' code is required try and isolate it and document it well (it can be attractive as a challenge for some people)
* leave 'rough edges' for people to fix (TODOs in code)


## Related links

* https://whatcanidoformozilla.org/
* https://www.joshmatthews.net/bugsahoy/
* https://starters.servo.org/
* https://www.joshmatthews.net/oscon16/
