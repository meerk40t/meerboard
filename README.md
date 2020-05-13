# meerboard
Streaming Style Laser Board (Arduino Compatible)

Holding place for a hardware Arduino-compatible M2-Nano compatible laser board. The idea is to build a streaming style laser board with Arduino hardware. The reason why Arduino laser controllers suck and crash is that they are trying to implement the GRBL stack and that's simply too complex and gcode is too cumbersome. The M2-Nano runs a 40 year old CPU but still runs at great speeds. Arduinos are cheap and easy to program. Thus the genesis of this project: the Meerboard (name not fully decided)

Initially phase one will be to just make a compatable board. It doesn't need to be fully compataible, but should generally work with software that runs M2 Boards at least initially.

For more explanations of the code there:
https://github.com/meerk40t/meerk40t/wiki/Lhymicro-GL

There are however, only 12 operations that the board needs to do.

* U/D: Changes the on-state of the laser
* T/B: Changes the direction-state of the x-axis stepper motor chip
* R/L: Changes the direction-state of the y-axis stepper motor chip
* T/B/M: Changes whether the ticks are suppressed to x-axis
* R/L/M: Changes whether the ticks are suppressed to y-axis
* [a-z]|\d{3}: Appends values to a distance magnitude register
* V\d{7} Sets the tick rate to the stepper motor
* N, any command in compact: Executes the current command state
* S1, S0: Change the state of whether the programmed tick rate is sent to the stepper motor chips.
* I: Resets the board
* P: Pause device
* PP: Home device

Mostly the byte stream just causes state changes in the stepper chips. These have a forward and backwards mode and tick along with. This is what the directional commands change T/B/R/L do. They set the direction setting on the stepper motor chip directly. They trigger whether that stepper motor is turned on. So T/B/M turn on the x-stepper chip and R/L/M turns on the y-stepper chip. So M commands turn on both stepper chips at the same time but do not change the directional setting on the chips themselves.

The N in default and *any* command in compact mode causes the distance execution to trigger. So if you give a distance like zzzz (1024 mils) the next command be it U/D/R/L/T/B/F/@ will apply that distance in whatever direction is set on the stepper chips. There's not much else to the chips. And they have weaker stepper chips than you'd find on modern devices which are notably louder.

---

While there are other things to do, this is the holding place for the idea. Having a working hardware would then be able to do things like give an alternative channel for a rotary output or manual control over a zbed, or PWM direct power control. These could be done by controlling another stepper chip, or applying another tick-stream to the power output.

Since most of these commands are trivial and all the real work is done on the computer system itself, this should allow for a simple base to get most of the same power and oomph out of a little arduino that you'd get out of several hundred dollars of hardware. Most of that hardware is largely for doing planning and gcode interpretation on the board but that can be done on the computer doing the real work, or on the computer that made the relevant file.

---

The basic idea of just transfering direct state changes to that board cuts off like 99.8% of the processing requirements for a laser controller, and there are off the shelf solutions for some of the other code elements.

If you have any ideas or suggestions in this vein, go ahead and raise an issue.
