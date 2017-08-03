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
works everywhere. GDB and LLDB are command
line tools, but can have frontends.

Debugger support can come in various forms in this architecture:
* we could invent our own debugger and debug symbols (this is probably infeasible
  due to resourcing)
  - There are experiments using MIRI for debugging - https://github.com/oli-obk/priroda
* generate better debuginfo in the compiler - most of the low hanging fruit here is done
* extend debug symbols with Rust-specific forms
  - DWARF is designed to accomodate this
  - would need support in GDB/LLDB
  - we would like to change the representation of some Rust items in non-back-compat ways
  - from mw: "My current thinking is to add a compiler flag, -Zrust-dwarf or
    something, that allow to opt into the new format, and start making changes in
    LLVM, GDB, and LLDB as needed. The debuginfo generated here would only be
    expected to work with our branches of debuggers initially. At the same time
    the old format is still emitted by default until we are ready to flip the switch."
* improve the debuggers handling of Rust
  - plugins (I think this is primarily an LLDB thing)
  - land code in the debuggers (done to some extent in GDB)
  - improve the debuggers *and* the debug symbols
  - pretty printers (used by debuggers to display rust data, see gdb and lldb
    pretty printers in https://github.com/rust-lang/rust/tree/master/src/etc)



## Pretty printers

* need to run via the scripts (in https://github.com/rust-lang/rust/tree/master/src/etc))
* still needed even with upstream improvements
  - not sure if there is conflicts
* gdb and lldb have quite different APIs


## Visual Studio support

* we support as much as llvm, but llvm is incomplete
* no CI for testing this
* http://www.jonathanturner.org/2017/03/rust-in-windows.html
* Visualisers can be used to improve the experience for key types like `Vec`
  - https://docs.microsoft.com/en-us/visualstudio/debugger/create-custom-views-of-native-objects

## Potential improvements

### Debug info

Better support for trait objects and method calls.

From tromey on trait objects: "for printing trait objects, only a minor DWARF
extension is needed; specifically, a way to link a vtable to the concrete type
for which it was created. That is, no new tag is needed, just the ability to put
a DW_AT_containing_type on the vtable object's DIE.".

Extending DWARF with Rust-specific forms, would need to upstream stuff to understand it (e.g., calling convention, enums).

* compiler emits new stuff
* modify LLVM (type layouts prob easy, maybe some optimisation issues)
  - LLVM must know about all debuginfo attributes and tags, it can't just 'pass through'
* modify lldb + gdb (if it is lang-specific or in a plugin, then probably doesn't need to be standardised)
* maybe some day standardise
* graceful degradation - llvm can't support different versions of debuginfo in the same binary, the user must choose the format at compile-time.
  - how to handle older debuggers or if, e.g., GDB gets ahead of LLDB?


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

### Executing statements during debugging

GDB supports evaluating C statements and running them on the target (e.g. `*0x1247 = 42`)
We could leverage miri to compile Rust statements, interpret them and run all address accesses through GDB.

## Misc

### ref/ptr

We should encode references as C++ references, not pointers

Might be back compat with pretty printers


### Buggy debuginfo for inline function
    
* Is this llvm or us?
* MIR inlining - are we adjusting debuginfo?


### lldb plugin

Some work underway - see https://docs.google.com/document/d/1a2v0ro40vy3z-Np44CF-5v6THH9Huy_Dtp8-aUIdgkM/edit

TODO what does this mean?

lldb C++ thing probably fixed
