# IDE planning minutes

## IntelliJ

* Get in touch re edition


## code completion

* re edition, ensure path completion continues to work
* not happening soon
* what about Racer?
  - get commit access for Xanewok + nrc
  - move to nursery? - yes
* can we have a partial solution?
  - impl info first
  - dot completion vs free text completion
  - where do we need to be perfect vs approximate
* how do we prioritise vs feature work, etc.
* what does the long term solution look like?
  - can we work towards that and provide short-term wins?


## post 1.0

features, performance, stability, tech-debt, maintenance

* testing
  - compiler
  - rls-analysis
  - rls
    * need more
    * declarative framework
      - message ordering
      - comments for spans
      - handle multiple files
      - for codegen etc. have before and after text
      - IntelliJ found easier to have rust files as inline files rather than one file each like in compiler; keeps everything in memory, not using Cargo, etc.
* stability
  - single file support
  - edition transition
  - debug mode
    * ring buffer of requests/responses
    * select what is most useful logging
    * backtrace when we crash
    * dump to file
    * emit issue template
    * auto crash reporting?
      - integrate with compiler telemtry?
    * more warnings/error messages to users?
      - just short warning, no why
      - no backtrace
      - show command line
* features
  - symbol tree in VSCode?
  - use LSP goto impl
  - single file support
  - code generation
    * generate match
    * generate function stub in an impl
    * generate impl for trait (p-high) (full or partial)
  - refactoring
    * extract local var
    * extract module (or crate)
      - create module (including convert file to directory)
  - auto imports (without errors)
    * mass import rather than clicking multiple lightbulbs
    * auto-remove unused imports
  - selection by AST node (does LSP have support) aka extend selection
  - new nested error support
  - test support in RLS or client?
    * run a single test (need to query RLS for the package)

## Prioritisation

* test infrastructure
* debug mode
* Racer is OK, so maybe not so important to do code completion - also the good solution is hard work
* pick low-hanging feature work
