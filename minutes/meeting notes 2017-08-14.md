# August 14th 2017

Agenda week, rustdoc rewrite

## Triage

We accepted https://github.com/rust-lang/rust/pull/43782 for uplift to beta.


## Background

* repo: https://github.com/steveklabnik/rustdoc

## Notes

Steve gave a brief status update and I failed to take notes, sorry.

### Triaging current Rustdoc capabilities

What can we break?

* can we remove rustdoc plugins?
  - all: what even are these?
  - https://github.com/rust-lang/rust/blob/master/src/librustdoc/plugins.rs
  - load dynamic libs at runtime
  - pluggable rustdoc passes
  - unstable - compiler internals (rustdoc internals)
  - ACTION: let's do a Cargo bomb run, then remove

* command line flags
  - in general - warning cycle and remove them
  - passes - can effectively be replaced by an explicit 'document private items' flag
  - add CSS file - alternative - template directory
    * motivtion is to change the colours of docs
    * deprecation  warning, but don't remove from current
  - playground URL flag - if you don't give it, don't get run buttons
    * default to rust-lang playground?
    * problem if you're not one of the top 100 crates
    * wait and see on this one (do we need to warn about anything? Steve to talk
      to infra team about playground future)
    * there's also a directive to use in the source

  - do not generate Table of Contents
    * only applies when run on a markdown file, not Rust source
    * used in reference
    * do people use the markdown documenting feature at all?
      - Gankro? For his blog. Others like this?
      - docs.rust-lang.org
      - used to test code in markdown
      - some crates (e.g., nom) splice markdown *into* docs
  - `--input-format`
    * basically ignored atm

### Feature request: show which functions can (transitively) panic

Unclear if we can even implement this. Also controversial as to whether this
would actually be useful.

### Theming

Current plan of record is to support CSS themes in a directory that the client
searches, and presents a menu for the user to pick a theme.

* mdbook does this - https://doc.rust-lang.org/book/second-edition/ 
* good for minor changes
* we provide some themes (e.g., dark theme, high contrast/accessible); users can
  extend with their own themes
* if we pull in mdbook, theming should be unified (and also with rustw)

For more serious presentation changes, write your own frontend.

### Markdown

Currently in the new rustdoc, markdown is rendered in the client. CommonMark
spec compliant, should be compatible with Pulldown, therefore, no repeat of the
current warning cycle issues.
