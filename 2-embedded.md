## Embedded Systems

* Systems too small to run a "real" operating system?

* Systems where the hardware is manipulated "directly"?

* IDK. We are working with the BBC Micro::Bit v2 (MB2).
  It is *definitely* a high-end embedded system.
  
## "High-end"??

* The MB2 has 512KB flash, 128KB RAM, runs at 64MHz. It can
  run MicroPython, although we will never speak of it here.

* The peripheral set includes some neat stuff. We will poke
  at much of it.
  
## MB2 documentation

* This
  [page](https://spivey.oriel.ox.ac.uk/baremetal/The_microbit_page)
  from Mike Spivey's Bare Metal Micro::Bit book (which I
  can't find anyplace).

* The [MB2 Hardware
  Documentation](https://tech.microbit.org/hardware/) is
  really excellent.

* We will occasionally refer to the [NRF52833
  manual](https://infocenter.nordicsemi.com/pdf/nRF52833_OPS_v0.7.pdf).

* We will be following the [Rust Embedded Discovery
  Book](https://docs.rust-embedded.org/discovery/microbit/)
  for as far as it takes us. (Not that far.)

## About Our Processor

* The MB2 wants the `thumbv7em-none-eabihf` target.

* `thumbv7em` is a version of the ARM ISA targeted at
  Cortex-M4 and Cortex-M7 processors. The ARM ISA is
  subsetted M0, M3, M4, M7, etc. The Thumb instruction set
  is a compacted form of the ISA for use when
  memory-limited: it trades more decode for smaller program
  size.

* `eabihf` is the Extended ABI with Hardware
  Floating-point. Yes, we have an FPU on this chip.

## Cargo config for embedded

* Rust is special in that it is *really* well set up for
  cross-compilation to embedded targets.
  
* The [probe-rs](https://probe.rs/) project is set up to
  enhance these capabilities further. [Ferrous
  Systems](https://ferrous-systems.com/) is a thing.
  
* Let's take a look at a sample Rust program and talk about
  its Cargo config a little:
  <https://github.com/pdx-cs-rust-embedded/blinky-rs>

## Linkers and other nightmares

* One thing the Rust tooling hides from us in part is
  linking/loading. Once the compiler is done, all that
  program code, initialized data, etc has to be set up in a
  form that the MB2 bootloader can take.

* Our tooling invokes `flip-link`, which is just a script
  that invokes the host platform's ARM linker, or `rust-lld`
  which is a link to the LLVM linker. These days most
  linkers will work with many ABIs including ARM.
  
* Everything expects ELF now. ELF is a format for object and
  executable files. The program loading tool will read info
  from the ARM executable and set the bootloader up to put
  things in the right place.
  
* "The right place" is pretty much per-board. Rust uses a file
  `link.x` to figure that out: it lives in the `target`
  directory until needed. Let's take a look…

---

## Bootloading and debugging

* There's a little bit of bootloading in ROM on a separate
  nRF52820. Essentially, there's some support for on-chip
  debugging, and this includes something that can take
  a binary by USB and put things where it's told.

* The specific protocol here is
  [CMSIS-DAP](https://developer.arm.com/documentation/101451/0100/About-CMSIS-DAP). Mostly don't ask.

* For Rust, the `probe-run` command should be installed, and
  sets this all up. The `cargo-embed` utility is also
  involved often.

* A more generic solution is provided by
  [OpenOCD](https://openocd.org/). For other boards, and
  other languages, this is an invaluable tool. We likely
  won't use it here.

## The Rust Embedded software stack: HAL

* Lowest-level is CPU support and Peripheral Access Crate
  support. In our case, these are provided by the
  [`nrf52833_hal`](https://crates.io/crates/nrf52833-hal)
  Hardware Abstraction Layer (HAL).
  
* The HAL is mostly generated mechanically by `svd2rust`,
  which takes a
  [CMSIS-SVD](https://www.keil.com/pack/doc/CMSIS/SVD/html/index.html)
  file. The System View Description (SVD) is effectively an
  XML summary of the microprocessor manual: it contains the
  organization and magic numbers for the chip. Here's [an
  example](https://www.keil.com/pack/doc/CMSIS/SVD/html/svd_Example_pg.html).

## The Rust Embedded software stack: Board Support

* Next level is a Board Support Crate. We are using the
  excellent
  [`microbit-v2`](https://github.com/nrf-rs/microbit) crate
  for this.

* The Board Support Crate provides higher-level
  interfaces atop the HAL. Think of these as "peripheral
  driver-ish things" and also more Rustic APIs.

## The Rust Embedded software stack: Embedded Runtime

* Finally, an Embedded Runtime can be dropped
  atop this.
  
* We will use the
  [RTIC](https://rtic.rs/2/book/en/) runtime pretty early on.

* We might also look at
  [Embassy](https://github.com/embassy-rs/embassy), which
  brings async/await to embedded (thanks?).

## The Rust Embedded software stack: Embedded OS

* Rather than a runtime, might want to go to a genuine
  Embedded OS. We will take a look at things like [Tock](https://tockos.org/)
  and whatever else strikes our fancy.
  
## The idiosyncratic Rust embedded stack

* The folks driving embedded Rust are *very* excited about
  Rust's capabilities.
  
* Rust traits, modules, lifetimes and ownership are used to
  protect embedded devs from mistakes. Sometimes mistakes
  that would never happen.
  
* We will talk soon about the philosophy and details.
