# Stability - rustfmt

This document enumerates areas where backwards compatibility could be a concern
and some possible quality measures for Rustfmt. For background on what stability
means for tools in general see [README.md](README.md).

## Backwards compatibility

* Output of formatting

I don't think we can promise 'no changes' without it preventing us fixing bugs.
I think we can promise that there will be no changes which violate the RFC/spec.
That would leave us some wriggle room in the ambiguities. It raises the question
of what happens if we change the spec. I hope we finish the spec before Rustfmt
1.0 and any changes after that (beyond 'bug fixes') are a breaking change for
Rustfmt.

I think we should only make promises for the default options. No promises at all
if you're using any non-default options.

* Command line args (`rustfmt` and `cargo fmt`)

I think we need a notion of stable vs unstable arguments with the obvious back
compat guarantees. Presumably, unstable arguments should only be usable on the
nightly channel.

An important argument is the `write-mode` that Rustfmt operates in - this
controls the output and what is done with it (overwrite files with or without
backup, pipe to stdout, etc.). We should extend stability to these and promise
that stable modes will not be removed, nor their operation changed.

The current default write mode is `replace` which leaves a backup, however,
before stabilisation we should change to `overwrite`, which does not. We should
then promise not to change the default again.

* exit codes

For stable modes, any change to the possible exit codes must be a breaking
change. The same code should continue to give the same exit code.

* Configuration options in rustfmt.toml and their defaults

We promise not to remove options, but we can change their effect, including
making them have no effect at all. We can deprecate options which means we stop
documenting them and warn on use. Alternatively we could also allow removing
options and not warn/error if using an unknown option.

* Library API

I would like to stabilise the public API of the Rustfmt crate with the usual
meaning for a library. I'm not happy enough with the current API to do it right
now though.

* skip attributes

Rustfmt can be made to skip sections using an attribute. I would prefer not to
stabilise the attribute we use before we settle how custom attributes work
(https://github.com/rust-lang/rfcs/pull/1755). However, I don't want to block
stabilisation on this. If we get to 1.0 before the custom attributes RFC is
decided, then we'll just support all supported attributes forever.

* warnings

Rustfmt gives warnings on overlong lines and trailing whitespace it can't fix,
and could add other warnings. These trigger a failing exit code in some modes. I
think we must promise not to add or remove warnings without opt-in of some kind.


## Quality

### Bugs vs formatting errors

We classify a bug as crashing Rustfmt or producing code which fails to compile,
or compiles but has different semantics from the source.

Formatting errors are where Rustfmt succeeds, but produces code which does not
match the RFC style guide or exceeds the width guide (with the exception of
comments or string literals, or exceptional situations such as very long
identifier names).


#### Comments and multi-line strings

Currently, comments and string literals are only formatted when some non-default
options are set. Ideally we would format these things, but in practice there is
often semantic info in their formatting and Rustfmt screws it up. It's not clear
if there is a better compromise than the current 'do as little as possible'
approach.

A second issue is that due to the nature of the AST (and architecture of
Rustfmt) we sometimes miss and thus erase comments in certain positions. Fixing
this will take either a rewrite of Rustfmt, a rewrite of libsyntax, or using a
different parser and AST (which would effectively mean a rewrite of both). I
don't think this is going to happen, and yet this is a major flaw in Rustfmt
right now.

I think we can make no promises about which comments we preserve since that
depends on libsyntax which is perma-unstable.


### Performance

How long does Rustfmt take to run on large code bases? It is currently a bit
slow. I don't think this is a problem for users except on very large projects
(Servo or rustc scale).

I don't think this should block any milestone, but we should keep an eye on it
and collect some data.


### Testing

I've found running on large projects (e.g., Rust) to be a good stress test. I
haven't done this for a while and doing this again should probably block a 1.0
announcement.

Rustfmt's test suite is fairly comprehensive, but is not well-organised. I don't
think there are any blocking issues there.


### Customisability

For which options do we apply the quality measures? I'm inclined to only care
about the defaults and to be explicit about this. I think we need to ensure that
we are offering the right set of options and none of them are too buggy. I don't
think there is any hope of ensuring quality for all combinations of options.


### Unicode wide char support

Rustfmt is currently not great with unicode - we use `byte`s in a bunch of
places we should use `char`s. I believe this won't cause us to crash anywhere,
but it will mean sub-optimal formatting and perhaps incorrect line-length
checks.


### Other important issues

* range support (important for IDEs, not so much for command line editors)
* warnings for skipped sections (i.e., if a section is skipped, should we warn
  on over-long lines, etc.?)


## milestones

### Inclusion with 'Rust product'

* agree on stability criteria


### stable channel

* tests running and passing on CI
* overwrite mode by default
* implement stable/unstable command line arguments
* resolve 'warnings for skipped sections'


### stable

* audit command line options
* audit config options
* RFC style fully implemented
* audit exit codes
* TBD - bugs, performance (at least collect data), skip attributes
* Library API is stablised


## Appendix - Rustfmt options and exit codes

```
Options:
    -h, --help          show this message
    -V, --version       show version information
    -v, --verbose       print verbose output
        --write-mode [replace|overwrite|display|diff|coverage|checkstyle]
                        mode to write in (not usable when piping from stdin)
        --skip-children 
                        don't reformat child modules
        --config-help   show details of rustfmt configuration options
        --dump-default-config PATH
                        Dumps the default configuration to a file and exits.
        --dump-minimal-config PATH
                        Dumps configuration options that were checked during
                        formatting to a file.
        --config-path [Path for the configuration file]
                        Recursively searches the given path for the
                        rustfmt.toml config file. If not found reverts to the
                        input file path
        --file-lines JSON
                        Format specified line ranges. See README for more
                        detail on the JSON format.

Exit Codes:
    0 = No errors
    1 = Encountered operational errors e.g. an IO error
    2 = Failed to reformat code because of parsing errors
    3 = Code is valid, but it is impossible to format it properly
    4 = Formatted code differs from existing code (write-mode diff only)
```
