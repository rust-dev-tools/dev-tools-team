# Stability - rustfmt

WIP

## Backwards compatability

* Output of formatting
  - expected major changes soon due to style RFC process
* Command line args (`rustfmt` and `cargo fmt`)
* Accepted options in rustfmt.toml and their defaults
* Library API


## Quality

### Bugs vs formatting errors

Classify a bug as crashing rustfmt or producing code which fails to compile, or compiles but has different semantics from the source.

Formatting errors are where Rustfmt succeeds, but produces code which does not match the RFC style guide or exceeds the width guide (with the exception of comments or string literals, or exceptional situation such as very long identifier names).

### Performance

How long does Rustfmt take to run on large codebases

### Testing

run on large test projects, e.g., the Rust repo

Rustfmt's test suite

### Customisability

For which options do we apply the quality measures


## milestones
