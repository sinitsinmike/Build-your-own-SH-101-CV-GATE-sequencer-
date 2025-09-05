# Build-your-own-SH-101-CV-GATE-sequencer-
Build your own SH-101 CV/GATE sequencer
Build your own SH-101 CV/GATE sequencer for just 1200 yen - modular synth

twenty one
HAGIWO
HAGIWO
August 21, 2021 03:13
I used Seeeduino xiao to create a CV/GATE sequencer similar to the Roland SH-101 modular synthesizer, so here are my notes.


background
This is my 29th modular synth creation.
There are many different types of modular synth sequencers. One of them is a Roland SH-101 type sequencer. You input phrases in advance using a keyboard, and then play them back in sync with the clock.

Image 2
The SH-101 type sequencer is used in standard modules such as Arturia keystep, mutable instruments yarns, and Intellijel Designs Scales.

I wanted a module that could "play any melody," which led to this project.

The module we are creating this time is also versatile, allowing it to be changed into various modules by changing the software. It has 1 gate input, 2 CV inputs, 4 CV outputs, and is operated by a rotary encoder and OLED, making it highly versatile.

Image 3
Production specifications
Eurorack standard 3U 6HP size
Power supply: 30mA (at 5V) 
Can operate on a single 5V power supply.

Interface

For step input (REC)
rotary encoder: Turn left to return to the previous step. Turn right to input a rest.
PUSH SW: Ends REC
CLK IN: (Unused)
IN1: Used as CV IN during step input (REC).
IN2: Used as GATE IN during step input (REC).
OUT1: GATE output for CH1 during input
OUT2: CV output for CH1 during input (10bit)
OUT3: GATE output for CH2 during input
OUT4: CV output for CH2 during input (12bit)

In the case of step output (PLAY),
rotary encoder: menu switching,
PUSH SW: menu selection,
CLK IN: outputs the next step when input,
IN1: (unused)
, IN2: (unused)
, OUT1: CH1 GATE output during input
, OUT2: CH1 CV output during input (10-bit)
, OUT3: CH2 GATE output during input
, OUT4: CH2 CV output during input (12-bit)

Image 5
Sequencer function
Number of steps: Max. 128 steps
Number of output channels: 2 channels
Output voltage range: 0 to 5V
Input voltage range: 0 to 5V

When a sequence output
clock is input, the CV/GATE of the next step is output.

If you select RESET, the step will return to the beginning.
If you select MUTE, the step will proceed but no GATE will be output.
If you select STOP, the step will not proceed even if a Clock is input.

You can select the divide rate with Clock Divide. The step will advance when the specified number of clock inputs are received.

Connect CV to sequence input
IN1 and GATE to IN2, and input one step at a time using an external input device (for example, Arturia KeyStep). Record the voltage at the moment the GATE switches from High to Low as the step.
Turning the rotary encoder right while inputting will input a rest. Turning it left will restart the input.

Image 4
Production cost
Total price: about 1,200 yen
------------------
Seeeduino xiao 550 yen
OLED (SSD1306) 180 yen
op-amp 30 yen
panel 100 yen
rotary encoder module 100 yen
DAC module (MCP4725) 150 yen
others

programming
It's a simple device that memorizes external input and outputs it according to the clock.

This time I used Seeeduino xiao instead of Arduino nano, but I was able to reuse the libraries for Arduino as they were.

The Seeeduino Xiao has a 3.3V rated voltage, so it will be damaged if a 5V CV input is used. Therefore, a resistor is used to divide the voltage, but the resistor error causes an error in the input voltage.
This time, we prepared a calibration variable to eliminate this error.

float AD_CH1_calb = 1.085;//reduce resistance error
float AD_CH2_calb = 1.094;//reduce resistance error

copy
Hardware
Since the Seeeduino Xiao's rated voltage is 3.3V, the input needs to be converted from 5V to 3.3V, and the output needs to be converted from 3.3V to 5V.
As mentioned above, input calibration is implemented in software.
To fully utilize the output resolution, calibration of the non-inverting amplifier circuit is implemented in hardware (variable resistor) rather than software.

Image 1
The GATE output uses a transistor to convert from 3.3V to 5V. Care must be taken as HIGH and LOW will be inverted.
Since transistors are components with a high voltage rating, the Schottky barrier diode protection circuit has been eliminated.

The 1uF capacitors D1 and D2 are not used this time. They are reserved for future PWM CV output expansion.

Advertisement: Support open source projects
To continue this open source DIY modular synth project, we are looking for patrons through a service called Patreon.
We would be grateful if you could support us with as little as a cup of coffee.
We also provide content exclusive to Patreons.

Get more from HAGIWO on Patreon
creating DIY eurorack modular synthesizer
www.patreon.com
Source code
It's rough, but I'm releasing it. If you have any bad points, I'd appreciate it if you could let me know.
