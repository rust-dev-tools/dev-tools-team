# Tool checklist

This is a checklist for each tool to try and ensure a good experience for new
and existing contributors.


## The repo

- [ ] it exists
- [ ] it is in an official looking org (either rust-lang-nursery or a tool-specific org)
- [ ] a 'comment here to help' issue, linked from CONTRIBUTING.md
- [ ] has an issue label for "good-first-issue" (or E-easy), there are at least a handful of such issues


## README.md

- [ ] it exists
- [ ] instructions for building, installing, running the tool


## CONTRIBUTING.md

- [ ] it exists
- [ ] clearly linked from README.md
- [ ] covers the basics of filing an issue and submitting a PR, including a link to Git basics (e.g., TODO)
- [ ] covers the review process, including how to request a review, expectations for making changes
- [ ] instructions for building, running, testing (how to setup environment if required)
- [ ] how to debug the tool (logging, does println! work, how to use/attach debugger)
- [ ] links to docs and/or some docs inline
- [ ] how to add a test
- [ ] link to dev-tools contribution document (TODO)
- [ ] how to get help (including specific people to ping)
- [ ] reviewed by an outsider


## Documentation

- [ ] easy to find (linked from README.md, CONTRIBUTING.md, and crates.io, on docs.rs)
- [ ] overall architecture is described in depth
- [ ] README or doc comments for important or complex modules
- [ ] doc comments for key data structures and functions (ideally, for all API and important internal items)
- [ ] complex items are commented
- [ ] examples where appropriate


## Review

This list is more a guide for reviewers, rather than anything that can get ticked off. TODO links for advice on actually reviewing

* there are enough reviewers for the volume of PRs
* be nice/polite - in particular ask for changes, don't command people to make changes
* be clear why you want a change, at least for new contributors who might not be familiar with conventions or requirements
* be clear about the relative importance of review items - some things are blocking, some can be left for later, some things are nits, some are important
* be firm - don't accept code you're not happy with
* for some contributors (students, new Rust programmers), code review should be an excellent learning experience - don't be afraid to review in detail
* if things are getting 'bogged down' in review, send a PR to the PR and ask the author to review that
* say 'thank you' if you appreciate a PR
* for new contributors check if they want something to work on next
* encourage contributors to review code


## Miscellaneous

- [ ] project is easy to test/build/run (ideally just `cargo X`)
- [ ] project conforms to Rust conventions (run rustfmt for formatting, follows coding conventions, uses 'best practice' libs)
