<!---

This file is used to generate your project datasheet. Please fill in the information below and delete any unused
sections.

You can also include images in this folder and reference them in the markdown. Each image must be less than
512 kb in size, and the combined size of all images must be less than 1 MB.
-->

## How it works

This project implements a three-level gauge that steps between low, medium, and high.
Two D flip-flops store a 2-bit state (Q1 Q0): 00 = low (red), 01 = medium (yellow), 10 = high (green).
The unused state 11 is defined to go to medium on the next clock, so the circuit cannot get stuck.
The direction input DIR is on ui[0], and it drives the select line of two multiplexers.
When DIR = 1, the muxes pass the up logic: Q1 = Q1 XOR Q0 and Q0 = Q1 XNOR Q0.
When DIR = 0, the muxes pass the down logic: Q1 = 0 and Q0 = Q1.
Each rising edge on the clock (the Step button) moves the gauge one level in the selected direction.
The gauge does not wrap around. It stays at the top or bottom level until the direction is changed.
LED_R on uo[0] is Q1', and LED_G on uo[1] is Q1 XOR Q0.
Yellow is not a separate output: it appears when LED_R and LED_G are both high.
The design does not use the reset input, so the starting state at power-up is arbitrary.

## How to test

In Wokwi, start the simulation.
Set DIP switch 1 (DIR) ON to count up and OFF to count down.
Press the Step button to advance the clock and compare the LED with this table:

| State (Q1 Q0) | LED_R / uo[0] | LED_G / uo[1] | LED color |
| --- | --- | --- | --- |
| 00 | 1 | 0 | Red |
| 01 | 1 | 1 | Yellow |
| 10 | 0 | 1 | Green |

With DIR ON, the LED should go red, yellow, green, then stay green.
With DIR OFF, the LED should go green, yellow, red, then stay red.
If the LED starts on an unexpected color, press Step a few times so the circuit reaches a known state.

## External hardware

The Wokwi simulation uses one DIP switch (switch 1 as DIR), one pushbutton (Step, on the clock input), and one RGB LED.
The RESET button from the template is wired to rst_n, but the design does not use it.
The LED is common cathode: the red pin connects to OUT0, the green pin connects to OUT1, and the common pin connects to ground.
The blue pin is unused.
These components are simulation controls and indicators outside the chip.
No external hardware is needed to generate or inspect the GDS file.
