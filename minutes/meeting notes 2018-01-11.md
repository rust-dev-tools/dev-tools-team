# January 11th, 2018

Agenda week, Vidyo meeting: Testing and custom test frameworks

## Notes

See also: https://internals.rust-lang.org/t/past-present-and-future-for-rust-testing/6354


### Scope

Output is really important and somewhat urgent, affects all kinds of tests.

Let's focus on the core test stuff first:  unit and integration tests. Leave doc
tests, compile tests, etc. for later.


### Initial design and requirements

Key question: how do we discover tests? In the most general case, how does the
test harness know what to run - could be anything in the program.

Python and Java use general purpose reflection for discovering tests

When running tests, how do we generate `main`? Does this come down to just test
discovery? Can/should the harness just do the rest?


#### How might a test framework specify a test?

Annotations:

* `#[test]`
* `#[rustfmt::skip]`
* `#[test(asdf = dfsadsf)]`
* `#[test::suite]`
* `#[test_suite]`

Other ways of specifying a test:

* naming - `test_foo()`
* DI (see PyTest)
* declarative tests (see Catch)

In at least the Catch case, annotations are not enough and a test framework
system would have to be more flexible.


#### Separating concerns

Running tests vs test output vs declaring tests.

Consensus was that unifying specifying and running tests probably makes sense,
but handling output is a separate issue.

API for output handling - event stream (see JSON test proposal)

* pluggable formatters
* could be an API or use JSON
* internal vs using RPC vs stdout-based API


### Benchmarking

* same model as tests - output API vs runner
* could it be a test runner?
* do we need to stabilise `#[bench]`
* `extern crate test;` to opt-in to bench?
  - note that `extern crate` might disappear in the future
* is benchmarking essential like unit testing?

### JSON PR

We should land as unstable, i.e., no intention to stabilise. This will let us
experiment and get some initial feedback on the API. We should note the intention
on the tracking issue.

### A few post-meeting thoughts

* rustup installable tools changes the landscape of built-in vs ‘library’
* `#[test]` is subtle and complicated because of compiler/Cargo considerations:
  interaction with #[cfg(test)], dead code lints, etc.


## Agenda

scope

* unit tests
* cargo integration tests
* benchmarking
* doc tests
* compiletest?
* other stuff - fuzzing, PBT (property based testing)? etc.
* coverage
* output

strategy

* support external test 'runners'
* anything we need to build in?
  - extensions to #[test]/attributes
* harness vs framework

requirements

* IDEs - streaming results
  * call for review: json output https://github.com/rust-lang/rust/pull/46450

bench

* ready to stabilise? Outstanding issues?
* see https://github.com/rust-lang/rfcs/pull/2287

paper cuts
* sequentialising tests
* process/thread execution control
* should support no_std targets, i.e. targets where neither threading or unwinding is available
* associating output with tests
* return Err rather than panicking (facilitate ? use)
