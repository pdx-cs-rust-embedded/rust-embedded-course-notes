## State Machines

* Idea from your CS career, super-used in embedded.

* Set of states describing, well, state of system. Usually
  finite.

* Set of state transitions describing movement from state to
  state.
  
* Start state, but usually no end states because embedded
  programs run forever.

## DFAs

* Deterministic Finite "Automata" (state machine).

* Next state is *function* of current state and some input.

* NFAs can be transformed to DFAs via algorithms: DFAs are
  standard in embedded because they are trivial to
  implement.
  
  
## DFA Embedded: Why do we care?

* Complicated structured flow control is really terrible in embedded:
  hard to debug, hard to validate, lots of code per unit
  function.
  
* DFA uses less code space, is often easily hand-validated,
  can be more easily debugged.

## Example: Blinky DFA

* Really, our Blinky loop is fine.

* A DFA would also be fine, though.

## Example: HW 2 DFA

* My original HW 2 code was normal structured flow. The core
  loop was hard to validate and debug.
  
  * "Timers" (counters) were used to keep track of game
    pauses and flip button state. These had to be kept
    right, using logic scattered across the program.

  * Button state was checked in semi-random places and
    handled in funny ways.

  * Initialization was done in pieces from several places.

## HW 2 State Machine

* Replaced my tangled code with a state machine like this:

  ```rust
  /// Program states.
  enum State {
      /// State to start a new randomized board.
      Initing,
      /// State to wait `remaining` ticks before continuing.
      Paused { remaining: Counter },
      /// State to complement the board.
      Flipping,
      /// State to step the game. Will only accept the "flip"
      /// button if `last_flip` is 0.
      Running { last_flip: Counter },
  }
  ```

* Button input is now checked only when `Running`, using a
  `match` statement that is guaranteed to detect all
  possible button combinations and take `last_flip` into
  account.
  
* Compiler checks that all states and transitions are
  accounted for.
