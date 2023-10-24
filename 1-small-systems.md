# Intro To Small Systems and Embedded Programming In Rust
2023-06

## Syllabus Review

* Hardware purchases:

  * Buy a MicroBit2 from the Engineering Prototyping Lab
    (EPL) in the basement of FAB for $25 (or buy elsewhere)

  * You won't need this until week two, but get on it
  
  * There will be another $20-$25 or so of hardware
    later when we play with electronics: can borrow, but
    strongly encourage buying

* Requirements: will talk in a bit

* No course project: we will just do an extra homework and
  some labs instead. Of course, you *can* do a course
  project if you want to and we are happy to help and look
  at it — we will even give some undefined amount of "extra
  credit", whatever that means.

## What you need: a computer

* You can use the Linux Lab boxes if you don't
  have anything of your own, but would encourage you to
  get your own.

  Linux is definitely easy mode for this. If working on a
  Windows box, we would encourage exploring Windows
  Subsystem for Linux 2 (WSL2), which comes with your
  modern computer. That said, most things can be done on
  Windows. Mac should also work, but neither of us has one
  or has used one (in modern times, ask Bart about the 128K
  Mac and/or Apple Lisa), so you're more on your own there.

## What you need: some basic Rust.
  
* If you don't have *any* Rust please work quickly
  through the first six chapters of [The Rust
  Programming Language](https://doc.rust-lang.org/book/).

  Rust is syntactically quite similar to other C-like
  languages, and semantically similar to C++ or Java but
  more capable than either. We will be there to help
  you if you get stuck, so don't be discouraged: you can
  do this.

* Please make sure you have a working Rust installed on
  your target machine *ASAP.*
  [Rustup](https://rustup.rs) is where to go to get
  started.

## What you need: some *basic* computer systems knowledge

* You should be aware that CPUs have registers and RAM
  memory, part of which is used as a stack. You should be
  aware that CPUs execute machine instructions. You should
  have some idea of what these things are/do. Our CS 201 (or
  whatever it's numbered these days) should be *more* than
  adequate. We will refresh you on these things shortly.

## What you *do not* need

* Electrical / electronics knowledge. We will get you what
  little you need early. If you've never seen an electricity
  before, this course is still very much for you.
  
* Detailed knowledge about the inner workings of embedded
  systems. That's a big part of this course.
  
## "Small" and Embedded Systems

* This course dives into this world. It is a somewhat vague
  and ill-defined domain.
  
## Small Systems

* By "small system" we mean something that is considered
  small by current standards. We expect a modern computer
  to have many GB of RAM, a CPU operating in the multi-GHz
  range, a wide range of fancy peripherals like disks,
  fancy graphics and video, WiFi, etc. Honestly, most
  modern computers use the `x86_64` instruction set.

* The Raspberry Pi we will be pretending to use this week
  is at the upper end of small. It's powerful enough to
  run Linux and do some really nice stuff. Nonetheless it
  is an ARM box with limited RAM (most used by Linux),
  runs at roughly 1GHz (speedy!), and doesn't have much in
  the way of peripherals built in.

## Embedded Systems

* "Embedded system" is even trickier. These systems have
  several ranges:

  * The biggest and fastest might be able to run a fancy
    operating system like Linux. Even then, it will be a
    really cut-down version. Most run ARM, RISC V, or some
    other "special" instruction set.

  * The next tier is machines like the MB2 we will be
    using. We will discuss this machine later, but to
    summarize it's an ARM 32-bit 64MHz 128KB RAM 256KB flash
    machine.  As such, it will run either a very simple
    cut-down "embedded OS" or just "bare metal". The MB2 is
    a "System on Chip" (SoC), meaning that all the core
    functionality including many peripheral drivers are on a
    single chip.

  * The final tier is really tiny machines, with specs like
    32KB ROM 16KB flash. These may be SoCs, or may just be
    bare tiny CPUs. These include 16-bit and 8-bit machines.
    They are stupidly cheap and great fun. They are almost
    always run bare metal.

## Interfacing: Pins

* Another big distinguisher of small and embedded systems is
  that they usually can make use of wires connected directly to
  the CPU pins for interfacing. These "pins" allow direct
  electrical connection of signals to the computer, without
  intervening layers such as USB.
  
* Pins are fantastic for HW cost saving and for doing really
  custom things. They are also a giant pain. We will rule
  eventually rule them in this course.

## Documentation

* The docs for small and embedded systems are *special.*
  Really special. They are written in a shorthand that is
  somewhat comprehensible to electronics folks and somewhat
  comprehensible to low-level programmers.
  
* Some of these docs are *big*: [NRF52833
  manual](https://infocenter.nordicsemi.com/pdf/nRF52833_OPS_v0.7.pdf)

* The docs are written in a language of abbreviations. For
  example, the NRF52833 Low-power comparator is consistently
  referred to as LPCOMP. The doc leads with "0-VDD input
  range", "eight input options (AIN0 to AIN7)" and goes from
  there.

## Learning To Read

* Learning to read this kind of stuff is a *skill* and
  *takes time* to learn. Nobody "just reads" a manual like
  this unless they've read 20 similar manuals in the past.
  *Do not* be intimidated.

* We will read this stuff to you when needed, and explain as
  you go. You will pick up the skill of reading yourself as
  you go along. By the time we're done, you'll be a
  beginner. This is awesome.

## Some Rust You Will Need

* There's going to be some fairly "advanced" Rust in this
  course. Nothing as scary as async/await, but some
  low-level stuff and some slightly fancy Rust types.
  
* You will need to be familiar with:

  * How Rust ownership works. Here's a quick review…

  * Rust error handling. It is used throughout. Here's a
    quick review…
    
  * Rust generic (parametric) types. Here's a quick review…

  * Rust value-carrying enums (sum types). Here's a quick
    review…

  * Rust traits. Here's a quick review…

* You will need some other stuff, too. We will catch it as
  we go.

## Cross-Compilation

* The Pi and the MB2 are both ARM-32. Your host machine is
  `x86_64`.  The MB2 doesn't have an OS. You can't just run
  your host code on either of these machines.

* You *can* compile for the Pi on the Pi. This is miserable,
  though: it's just too small and slow. Do not recommend.

* The solution is "cross-compilation": compile programs to
  ARM-32 on your host machine and then move them over.
  
* For many languages, cross-compilation isn't even possible.
  For C/C++ it is, but it is just an amazing pain.

* For Rust, cross-compilation is a breeze. The standard
  tools support it. You tell Cargo that you want to build an
  ARM-32 version of your code, and it just… does. You move
  it over and run it.

## Demos and HW
