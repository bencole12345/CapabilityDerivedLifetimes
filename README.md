# Capability-Derived Lifetimes

This document describes the artefacts I produced for my Master's work on *capability-derived lifetimes*, and how to reproduce them.

## List of artefacts

I pushed all my changes to `stack-capability-derived-lifetimes` branches of the upstream repositories.

- [LLVM changes](https://github.com/CTSRD-CHERI/llvm-project/tree/stack-capability-derived-lifetimes) - I added a pass to insert new "check" instructions for stores, and updated the frame-lowering logic for the CHERI-RISC-V backend to make sure that stack frames get aligned during function prologues according to the new alignment requirements that the CDL scheme requires.

- [CheriBSD changes](https://github.com/CTSRD-CHERI/cheribsd/tree/stack-capability-derived-lifetimes) - I added a system call to revoke capabilities pointing to a specific stack frame, and updated the behaviour of the CHERI-RISC-V trap-handling logic to detect stack lifetime violations and handle them separately.

- [Toooba changes](https://github.com/CTSRD-CHERI/toooba/tree/stack-capability-derived-lifetimes) - I added support for the four new instructions.

- [QEMU changes](https://github.com/CTSRD-CHERI/qemu/tree/stack-capability-derived-lifetimes) - Same as Toooba, I added support for the four new instructions. I did most of the development work using QEMU, so it's likely that this will be more stable/bug-free than the Toooba one.

- [Sail model changes](https://github.com/CTSRD-CHERI/sail-cheri-riscv/tree/stack-capability-derived-lifetimes) - Same as above, I modelled the new instructions (as well as the new capability metadata format).

For each of these, I squashed all my changes into a single commit to make it clear which commits are my work and which are the work of others. See the git log or email me (bc465@cantab.ac.uk) if you're unsure!

## Tests and microbenchmarks

Here are the repositories containing the various tests that I used to verify correctness, and the microbenchmarks that I referenced in my dissertation.

Everything is in [one big repo](https://github.com/bencole12345/StackTemporalSafetyTests). The Makefile in this repository contains all the targets for building the binaries and copying them to the emulator. It also produces separate binaries for some of the tests - I used these to test out adding new instructions to binaries before I had implemented the relevant compiler changes to insert them automatically.

If you actually use this then you'll need to update some of the absolute paths I've hardcoded into that Makefile. Again, email me (bc465@cantab.ac.uk) if you want to use this and aren't sure what to do. To be honest, you'll almost certainly want to use this for nothing but inspiration for writing your own. I knew (and still know) very little about writing tests for this kind of thing. The tests mostly revolve around binaries that should or should not fail when executed, as indicated by their names. I don't have automated unit tests or anything like that.
