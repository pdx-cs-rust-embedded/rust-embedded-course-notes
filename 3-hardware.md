## Electricity

* Two things: Voltage and Current

* Two things: Direct Current and Alternating Current

* Two things: Digital and Analog


## Voltage and Current

* Voltage (volts): How hard the electricity is being pushed.
  10V ain't much. 100V will push enough through you to kill
  you. 1000V will start to push through air and wires.

* Current (amps): How much electricity is being pushed. 1A
  is more than our board will comfortably deal with: too
  much melts things. 100A is an arc welder.

* DC power supply tries to keep volts the same no matter how
  many amps are being asked for: if more amps are flowing
  out, it pushes harder to keep volts the same.

* Ground: A place where electricity goes to
  disappear. US symbol is an upside-down tree.

## Resistors and Linear Things

* A resistor limits current, turning excess into heat in the
  process. US symbol is an angle squiggle. Unit is ohm Ω.

* Our power supply will be 3.3V on one end and ground on the
  other always: current flows "in a circle" from 3.3V to ground.

* Ohm's Law: V = A × R (E = I × R) where V is volts, A is amps, R is ohms.

  That is, if you measure the voltage across a resistor, the
  current through it, and its resistance, this equation will hold

* Power Law: P = V × A where P is power in watts. Doing some
  algebra with Ohm's Law gives P = A^2 R = V^2 / R

## Kirchhoff Voltage and Current Laws

* All the current has to go somewhere: the sum of the
  currents into a wire is 0 (output = input)

* All the voltage has to go somewhere: along any path, the
  sum of the voltage differences is 0

## Simple Circuits: Voltage Divider

* Have some voltage V1, want V2 < V1 out. We can do a
  voltage divider. Let's drop our 3.3V. With some
  simple math…

* Note that this works only if the "load current" is small.

## Semiconductor Devices: Diode

* Symbol is arrow and bar.

* Does not let current flow backward. Has small constant
  forward voltage drop (usually around 0.3V).
  
* LED is just diode with larger forward drop (depending on
  color) with energy turned into light instead of heat.
  
## Semiconductor Devices: MOSFET Switch

* Funny symbol.

* Allows or does not allow current to flow from "source" to
  "drain" depending on voltage at "gate" terminal.

* Usually used with GPIO pin to switch large power on/off.

## Capacitors and Alternating Current

* So far, assumed that the circuit just sits in some steady state.

* Very common to have voltage/current vary sinusoidally (why
  sin? because). This is AC

* Capacitor (C, in farads) is device that blocks DC, passes
  AC. Higher frequency or bigger C passes easier.

* Capacitor is device that "charges up" to its input voltage
  (difference).
  
* Capacitors are for smoothing, filtering, timing etc.

## Digital and Gates

* Digital is binary: voltages are either 3.3V or 0, draw as
  little current as feasible.
  
* Gates are a thing:

        1 2 & | !2

        0 0 0 0 1
        0 1 0 1 0
        1 0 0 1 1
        1 1 1 1 0

* Digital circuits are a thing on our chip.

## Binary and Bits

* Binary (base 2), hex (base 16): numbering schemes for
  digital

        3 2 1 0 #  #h
        
        0 0 0 0 0  0
        0 0 0 1 1  1
        0 0 1 0 2  2
        0 0 1 1 3  3
        0 1 0 0 4  4
        ...
        1 1 0 0 12 c
        1 1 0 1 13 d
        1 1 1 0 14 e
        1 1 1 1 15 f
        
        2f = 0010 1111

* Hex bits join like binary bits

## Bitwise Operators

* Just boolean gates, but each bit treated separately

        0110 & 1101 = 0100
        0110 | 1101 = 1111
        !1101 = 0010

* "Turning on a bit": or with that bit position

        abcd | 0100 = a1cd

* "Turning off a bit": and with negation

        abcd & !0100 = abcd & 1011 = a0cd

* "Testing a bit": and

        abcd & 0100 == 0100 or 0000 ?

* Bit operators may matter in this course.
