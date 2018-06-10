# June 5th, 2018

## Notes

### triage

* Did you mean to block nightlies on clippy?
  - https://github.com/rust-lang/rust/pull/51122
  - Worried that this was getting ahead of ourselves with Clippy, but this does not block nightly, just makes Clippy available
  - Should use the never-tried functionality in Rustup for preventing an update if a tool is not present
  - let's land it!
  - Related: https://github.com/kennytm/rustup-toolchain-install-master and https://github.com/rust-lang-nursery/rustup.rs/issues/1429
    * Get the latest commit as a toolchain, rather than just nightly.

### move atom plugin (and others) to the nursery?

* Start with Atom since it is most advanced plugin
* Probably do other plugins later too
* VSCode is already there
* editor syntax highlighting is in rust-lang org.

### question about using root of scoped attributes 

* https://github.com/rust-lang/rust/issues/44690#issuecomment-392800598
* we'd use clippy::* or something
* doesn't need to block stabilisation
* Manish to comment

### discord/irc/gitter

* let's make some Discord groups
* start to move away from Gitter
* leave irc until the official announcement


### teams and working groups

* 1.26.1 release to fix rls target-dir issue
* lldb has expresson execution, needs testing. Will need to ship our own lldb with rustup
* unicode identifier RFC could affect tools
