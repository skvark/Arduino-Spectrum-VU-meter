Arduino based 6 x 8 RGB LED screen + FFT + Spectrum VU meter
============================================================

This project has already took me 2 months and I'm only done with the screen and electronics itself, code is still missing but hopefully it will be here sooner or later :)

I'll put this into separate Github page later.

__This is still a very early prototype. Proof of concept.__

If I get this all together and working, the screen will be mounted into a Boombox which is kind of a side project of this project :D

There's actually two distinguishable parts in the project and therefore I'll divide this short documentation into few parts:

1. Arduino, Fast Fourier Transform and controlling the screen (coming later)
2. Arduino and audio input, A/D conversion and preamplifier (coming later)
3. Arduino RGB shield, TLC5940's and lot's of PWM outputs
4. Part list

All the code, circuit diagrams etc. will be open source. Might take some time to get them all here.

**1. Fast Fourier Transform and controlling the screen**

I'm not so good at math, so luckily someone coded the FFT algorithm originally for C in 1989. 

After that it has been enchanced couple of times, the last modification was to adapt it for Arduino.
I used the code which can be found from [coolarduino blog] as a backbone for this project.
Thanks to the blog's writer for providing this awesome piece of code in public use.

Fast Fourier Transform is an algorithm which was developed for computing Fourier's transform faster. 
Basically discrete Fourier transform slices the input signal's frequency spectrum into equal sized parts and gives
the amplitude of each part. Human hearing is about 20 Hz - 20 000 Hz so I'll be using frequency blocks between those values.

FFT can be used naturally for many kind of animations but the most famous is the bar graph in which every bar represents one frequency block.
There's 6 bars in my screen, so I have to do some averaging or simply add some blocks together to squeeze many blocks into one.

I have 3 of my RGB shields stacked on top of the Arduino and each one of them controls 16 RGB leds, one IC's is for one color.

Every led is capable of displaying RGB-colors and all the combinations.
The values for addressing TLC5940 via Tlc5940 library are following:

*first bar*
redbar1 = 0 - 7
greenbar1 = 16 - 23
bluebar1 = 32 - 39

*second bar*
redbar2 = 8 - 15
greenbar2 = 24 - 31
bluebar2 = 40 - 47

*third bar* 
redbar3 = 48 - 55
greenbar3 = 64 - 71
bluebar3 = 80 - 87

*fourth bar*
redbar4 = 56 - 63
greenbar4 = 72 - 79
bluebar4 = 88 - 95

*fifth bar*
redbar5 = 96 - 103
greenbar5 = 112 - 119
bluebar5 = 128 - 135

*sixth bar*
redbar6 = 104 - 111
greenbar6 = 120 - 127
bluebar6 = 136 - 143

**2. Arduino and audio input, A/D conversion and preamplifier**

To get the FFT done properly, you'll need some kind of preamplifier to boost the signal. I used myself
circuit which can be found from [coolarduino](http://coolarduino.wordpress.com/2012/06/22/audio-input-to-arduino/).

The circuit uses only one channel, left or right. Since I'm using FFT only for aesthetic reasons for my Boombox, I don't need stereo signal.

The signal cable is connected to Arduino's analog input 0 from which an array is filled with values to perform the FFT. 

**3. Arduino RGB shield, TLC5940's and lot's of PWM outputs**

As you may have already noticed, Arduino isn't capable to control 144 leds independently. There's no way to do that without some help. To get enough PWM (Pulse Width Modulation) outputs, 
I had to get these little magical IC's called [TLC 5940](http://www.ti.com/lit/ds/symlink/tlc5940.pdf). Every one of those contains 16 PWM outputs. 16 * 9 = 144, so I have 9 pieces of them.

To get them all work together they have to be daisy chained. Basically that means you connect certain pins on first IC to the next and so on.

At first I thought I could solder everything to a prototyping PCB but it didn't work out too well. So the only possible way to get things going was to design a new shield for Arduino.

I'm not an electrical engineer, I have never drawn or done anything like this. Still I managed to design this shield for Arduino. It's not good, it's not very practical but it works. That's enough for me at this point.

There's some design flaws in the shield. You have to probably add more decoupling capasitors, I used 0,1 uF and 4,7 uF on each IC. 
I noticed also that adding 47 pF ceramic capasitor between XLAT and SCLK outputs on the last shield in the stack made the circuit a lot more stable and flicker free. Don't ask why :D

Every shield has daisy_in and daisy_out pinouts. I have no experience engineering this kind of stuff so to connect the pinouts to the next shield you have be creative.

On the first shield __there has to be jumpers__ in the another pinouts (near the edge of the PCB). Signal wont get to any of the shields if the first shield is not connected to Arduino outputs.
Leave these pinouts alone in other shields, no use for them after the first one.

I will design new revision of the board later, there's not enough holes for all the caps etc.

**4. Part list**

You'll need the following parts to build this thing (assumed 144 outputs, more or less shields can be stacked).
I recommend to buy more caps and resistors than the list suggests, cheap stuff.

- 9 x TLC5940
- 9 x 2 kOhm resistors
- 1 x 1 kOhm resistor
- 9 x 0,1 uF capasitors
- 9 x 4,7 uF capasitors
- 2 x 10 uF capasitors
- 3 x 47 pF capasitor
- 4 x 10 kOhm resistors
- 2 x 100 kOhm resistors
- Arduino Uno
- the shields from some place which fabricates them
- Arduino stackable headers
- some random headers for daisy chaining the shields together (or cable)
- cable
- 48 RGB leds or 144 normal leds
- [NE5532](http://www.ti.com/lit/ds/symlink/ne5532.pdf) for signal ampliflying
- power source (3,3-3,7 V) which can output 3 amperes (Yeah, 144 leds can draw some power)
- soldering tools etc.
- prototyping boards
- breadboards

More to follow...