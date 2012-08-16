Arduino based 6 x 8 RGB LED screen + FFT + Spectrum VU meter
============================================================

This project has already took me 2 months and I'm only done with the screen and electronics itself, code is still missing but hopefully it will be here sooner or later :)

So this is still a very early prototype. Proof of concept.

If I'll get this all together and working, the screen will be mounted into a Boombox which is kind of a side project of this project :D

There's actually two distinguishable parts in the project and therefore I'll divide this short documentation into few parts:

1. Arduino, Fast Fourier Transform and controlling the screen (coming later)
2. Arduino and audio input, A/D conversion and preamplifier (coming later)
3. Arduino RGB shield, TLC5940's and lot's of PWM outputs
4. Part list

All the code, circuit diagrams etc. will be open source. Might take some time to get them all here.

**1. Arduino, Fast Fourier Transform and controlling the screen (coming later)**

Will be done later...

**2. Arduino and audio input, A/D conversion and preamplifier**

Will be done later...

**3. Arduino RGB shield, TLC5940's and lot's of PWM outputs**

As you may already have noticed, Arduino isn't capable to control 144 leds independently. There's no way to do that without some help. To get enough PWM (Pulse Width Modulation) outputs, 
I had to get these little magical IC's called TLC 5940. Every one of those contains 16 PWM outputs. 16 * 9 = 144, so I have 9 pieces of them.

To get them all work together they have to be daisy chained. Basically that means you connect certain pins on first IC to the next and so on.

At first I thought I could solder everything to a prototyping PCB but it didn't work out too well. So the only possible way to get things going was to design a new shield for Arduino.

I'm not an electrical engineer, I have never drawn or done anything like this. Still I managed to design this shield for Arduino. It's not good, it's not very practical but it works. That's enough for me at this point.

There's some design flaws in the shield. You have to probably add more decoupling capasitors, I used 0,1 uF and 4,7 uF on each IC. 
I noticed also that adding 47 pF ceramic capasitor between XLAT and SCLK outputs on the last shield in the stack made the circuit a lot more stable and flicker free. Don't ask why :D

Every shield has daisy_in and daisy_out pinouts. I have no experience engineering this kind of stuff so to connect the pinouts to the next shield you have be creative.

On the first shield __there has to be jumpers__ in the another pinouts (near the edge of the PCB). Signal wont get to any of the shields if the first shield is not connected to Arduino outputs.
Leave these pinouts alone in other shields, no use for them after the first one.

I will design new revision of the board later, there's not enough holes for all the caps etc.

More to come...

**4. Part list**

You'll need the following parts to build this thing (assumed 144 outputs, more or less shields can be stacked).
I recommend to buy more caps and resistors than the list suggests, cheap stuff.

- 9 x TLC5940
- 9 x 2 kOhm resistors
- 1 x 1 kOhm resistor
- 9 x 0,1 uF capasitors
- 9 x 4,7 uF capasitors
- 2 x 10 uF capasitors
- 2 x 47 pF capasitor
- 4 x 10 kOhm resistors
- 2 x 100 kOhm resistors
- Arduino Uno
- the shields from some place which fabricates them
- Arduino stackable headers
- some random headers for daisy chaining the shields together (or cable)
- cable
- 48 RGB leds or 144 normal leds
- NE5532 for signal ampliflying
- power source (3,3-3,7 V) which can output 3 amperes (Yeah, 144 leds can draw some power)
- soldering tools etc.
- prototyping boards
- breadboards

More to follow...