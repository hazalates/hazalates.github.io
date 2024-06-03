---
title:  "Neopixel board"
---

A while ago i made a box with the CNC machine. I milled my pepper logo in the lid of the box, and already came up with the idea to make LED's behind it, so that every time the lid is lifted, the LED's will blink. Henk told me he wanted me to make a simple Hello World board first, before starting to mess around with Neopixels. Since the Hello World board is finished, it's time to blink some LED's!

## First design in Kicad

I made a first design in Kicad. 

I want to use the WS2812B Neopixel LED's because those do not require separate chips, capacitors and resistors. I want the board to work on a 9v battery, which i use a 1A LDO voltage regulator (the neopixels require around 60mA each) to convert to 5v. I'll use 2x 10uF caps around the LDO to smooth signal and a flipped LED to protect the board from short-circuit in low power conditions. I'll use UPDI to program, and the input sensor will be a photoresistor with a 10k pullup resistor to avoid floating. The ATtiny412 won't have enough flash memory for my program, so i'll use an ATtiny1614. I protect the chip with a 1uF cap. 

![neopixel](/assets/images/2022-06-22-neopixel-board/neopixel3.jpeg)

![neopixel](/assets/images/2022-06-22-neopixel-board/neopixel9.jpeg)

![neopixel](/assets/images/2022-06-22-neopixel-board/neopixel10.jpeg)

The boardsize is very large and would not fit on the sacrificial layer of the Modella. Also it would be a waste to spill so much copper.

## Second design in Kicad

To avoid spilling so much copper and to be able to first test the circuit i made a small test-circuit.

![neopixel](/assets/images/2022-06-22-neopixel-board/neopixel5.jpeg)

![neopixel](/assets/images/2022-06-22-neopixel-board/PCB1.gif)

The board is working, but i ruined the traces and the UPDI pins are upside down. But proof of concept is complete. 

## Third design 

Instead of making the full board out of copper, i used the lasercutter to cut out a acrylic plate to glue the neopixels onto. I used the pepper in the DXF of my CNC box, and used illustrator to add an offset. Next i glued the neopixels on the acrylic plate and used wire to solder them together. I used several acrylic plates on top of each other to dilute the light of the neopixels.

![neopixel](/assets/images/2022-06-22-neopixel-board/neopixel2.jpeg)

I drew a new PCB on top of the offset image of the pepper, adding a switch and a large capacitors before the neopixels, and inverted the orientation of the UPDI pins. 

![neopixel](/assets/images/2022-06-22-neopixel-board/neopixel6.jpeg)

Using Illustrator i added my name and signature to the board. 

![neopixel](/assets/images/2022-06-22-neopixel-board/pepper.png)

I also printed a battery holder using the Prusa and [this design](https://www.thingiverse.com/thing:336319).

![neopixel](/assets/images/2022-06-22-neopixel-board/neopixel8.jpeg)

During milling i had a slight error with the bed not being level. Leveling the z-axis again resolved this issue.

![neopixel](/assets/images/2022-06-22-neopixel-board/level.jpeg)

Board after stuffing: 

![neopixel](/assets/images/2022-06-22-neopixel-board/board.jpeg)

## Programming

This was my first time using an ATtiny1614, so i struggled a bit finding the right settings to program. I used the MegaTinyCore board lib and the 16Mhz internal clock settings. I used my dualserial with the serialUPDI - Slow programmer booted from the Arduino IDE. 

To test the board, i downloaded the Adafruit Neopixel lib and used the Strandtest example.