# July 31st, 2018

## Agenda

* Rustfmt RFCs
* rustfix checkin
* edition deadline reminders


## Notes

### Rustfmt RFCs

* interaction with Clippy? (i.e., overlap in coding style guidelines)
  - not much (only capitalisation, probably)
  - would be nice to perform a similar exercise for Clippy some day
* please test formatting nested imports
* FCP proposed - please comment and tick boxes

### Rustfix

* basically all good: a few minor issues, mostly already fixed
* lint filtering (https://github.com/rust-lang/cargo/issues/5738)
 
### Edition deadlines

* need to ride the next train for testing on stable.
* extended beta - https://internals.rust-lang.org/t/rust-2018-release-schedule-and-extended-beta/8076.
* some concerns about lints (https://github.com/rust-lang/rust/issues/52679).

### Testing

jrenner has been working on the test visibility bug (glob shadowing).

### cargo generate

* Is it a plugin or a new command?
  - plugin for now, hopefully command later.
* Who should look after this?
  - cargo team's business eventually.

### Clippy RFC

* still waiting on compiler team.
* Will leave clippy as preview for edition if it doesn't get resolved (not too
  bad, but need to message this well).

