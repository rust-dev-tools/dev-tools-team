# Debugging

This document aims to collect info on the current status and future opportunities for debugging Rust.

## Debugging architecture overview

The compiler generates debuginfo for Rust programs (see https://github.com/rust-lang/rust/tree/master/src/librustc_trans/debuginfo).
This is an LLVM-specific thing. LLVM then processes this through optimisations
(with varying degrees of success) and outputs debug symbols with the library.
These are DWARF for the Linux, Mac, and Windows/GNU toolchains, and pdb (program
database) for Windows/visual studio (pdb support is WIP in LLVM, I believe).

Debuggers consume the generated symbols. Visual Studio only works on Windows.
LLDB works everywhere but is primarily used as a backend for XCode on MacOS. GDB
works everywhere. Visual Studio only works on Windows. GDB and LLDB are command
line tools, but can have frontends.

Debugger support can come in various forms in this architecture:
* we could invent our own debugger and debug symbols (this is infeasible due to resourcing)
* generate better debuginfo in the compiler - most of the low hanging fruit here is done
* extend debug symbols with Rust-specific forms
* improve the debuggers handling of Rust
  - plugins (I think this is primarily an LLDB thing)
  - land code in the debuggers (done to some extent in GDB)
  - improve the debuggers *and* the debug symbols
  - pretty printers (used by debuggers to display rust data, see gdb and lldb pretty printers in https://github.com/rust-lang/rust/tree/master/src/etc)



## pretty printers

* need to run via the scripts (in https://github.com/rust-lang/rust/tree/master/src/etc))
* still needed even with upstream improvements
  - not sure if there is conflicts
* gdb and lldb have quite different APIs


## Visual Studio support

* we support as much as llvm, but llvm is incomplete
* no CI for testing this
* http://www.jonathanturner.org/2017/03/rust-in-windows.html


## potential improvements

### debug info

Better support for trait objects and method calls. TODO would this require DWARF extensions?

Extending DWARF with Rust-specific forms, would need to upstream stuff to understand it (e.g., calling convention, enums).

* compiler emits new stuff
* modify LLVM (type layouts prob easy, maybe some optimisation issues)
* modify lldb + gdb (if it is lang-specific or in a plugin, then probably doesn't need to be standardised)
* maybe some day standardise
* graceful degradation - llvm can't support different versions of debuginfo - how to handle older debuggers or if, e.g., GDB gets ahead of LLDB?


### Distribution improvements

Make std lib debug symbols available through infra (probably Rustup).

Can we inform GDB of the right std lib src (from Rustup)?

### Macro debug info

Currently:

* always jumps over macros
* buggy

We want an option to jump into macros.

Not a great fit for macros in DWARF. Could use inline function, but there might
be issues with local variables, etc. Adding our own stuff to DWARF would be
complex - it is pretty fragile around inlining.

Similar questions as to macros in error message. Should we allow authors to mark
macros as opaque? Or use the per-crate heuristic? (Only show info for same-crate
macros).

## Misc

### ref/ptr

We should encode references as C++ references, not pointers

Might be back compat with pretty printers


### buggy debuginfo for inline function
    
* Is this llvm or us?
* MIR inlinging - are we adjusting debuginfo?


### lldb plugin

TODO what does this mean?

lldb C++ thing probably fixed
