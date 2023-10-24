## Interrupts

* Since pretty much the dawn of CPUs

* CPU can be "interrupted" by:

  * Voltage change on some *external* wire: for example, button
    press changing GPIO input state, external peripheral
    signalling ready.
    
  * Voltage change on some *internal* wire: for example, HW
    timer expiring, UART needing attention.
    
## Interrupt Handling: Starting

* When a CPU receives an (unmasked) interrupt, it *suspends
  execution of the currently running code:*
  
  * Code execution is paused.

  * CPU saves CPU registers including Program Counter on the
    stack.

  * CPU looks up an entry corresponding to the interrupt
    (wire) number in an array of code addresses called the
    "interrupt vector table".
    
  * Whatever address is in the interrupt vector, the CPU
    jumps to there.

  * CPU is now running an "interrupt handler".

## Interrupt Handler: Completing

* When an interrupt handler completes, it (with some help
  from the CPU):

  * Restores the CPU registers (saved on stack) to what they
    were on handler entry.

  * Puts the PC back to its stored value in the main program.
  
* The main program continues as though it were never
  interrupted, but now maybe some memory is different.

## Re-entrancy

* An interrupt handler *may* interrupt other interrupt
  handlers.
  
* Most CPUs have an interrupt priority system: an interrupt
  may not be interrupted by its own interrupt or by another
  higher-priority interrupt.

* There is a highest-priority "non-maskable interrupt" that
  cannot be blocked or ignored. This handler does not
  complete: it is used to hard-reset the system.

## Concurrency

* Note that an interrupt may be taken between *any two
  machine instructions.*
  
* We thus have concurrent code: we have to treat the
  interrupt handler essentially as a separate thread.
  
* *...Or do we?* We can get "critical sections" by disabling
  interrupts while we do some work, on any code accessing
  shared data.

* The `cortex-m` crate provides facilities for this.

## Let's Handle An Interrupt

* So far, we've been *polling* our buttons: looking at them
  periodically to see if they've changed.
  
* This is awkward in code and not particularly reliable.

* Let's look at a button interrupt handler, stolen from the
  `microbit` crate examples:
  <http://github.com/pdx-cs-rust-embedded/gpio-hal-printbuttons>

* Some reference material from the *other* Rust Embedded
  Discovery Book:
  
  * https://docs.rust-embedded.org/book/start/interrupts.html
  * https://docs.rust-embedded.org/book/start/exceptions.html

* This material is a bit dated, but it's a great description
  otherwise.
