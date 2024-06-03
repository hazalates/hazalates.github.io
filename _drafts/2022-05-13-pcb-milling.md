---
title: "PCB milling with the Modella milling machine"
---

#### How to use the Modella PCB milling machine

![pcb](/assets/images/2022-05-13-pcb-milling/pcb2.jpg)

This is the Modella PCB milling machine.

The surface of the Modella consists of 4 layers

1 - Plastic see trough plate 

2 - Acrylic plate

3 - Sacrificial layer

4 - Copper plate

###### Copper plate

The copper plate is the layer from which the PCB will be made.

We mostly use one-sided copper plates, but we also have two-sided copper plates. (for double sided PCB's).

We don't use the green glass plates, since they will make a lot of mess and destroys the milling bit easily.

Always check the thickness of the copper plate with a caliper. This should be around 1.4-1.6mm.

###### Milling bits

We have a few different milling bits

Thickness: 

0.4mm - used for traces

0.8mm - used for outlines

Flutes: 

1 flute - standard

2 flute - used for very precise milling

###### Programmers

Before you can program your self-made PCB, you'll need a programmer, also called ISP (in-system programmer).

A programmer is a PCB specifically for programming µC's (micro-controllers).

There are different programmers for different µC's.

| Programmer | Micro-controller |
| --- | --- |
| JTAG | older SAMD |
| SWD | newer SAMD |
| UPDI | ATtiny |


#### How to mill a PCB

##### Setting up the Modella

1. Clean the sacrificial layer with sticker remover.

2. Pick a copper plate. Measure how big your PCB will be and if it will fit on the plate. 

3. Put double-sided tape on the bottom of the new copper plate. Make sure the whole surface is covered, but the tape can not overlap. This will create a level difference resulting in uneven milled traces.

4. Use a piece of fabric to press the copper plate to the sacrificial layer (no dirty fingerprints on the plate). Press tightly but don't overdo it. 

5. Move the mill head up with the up button on the Modella before changing the milling bit. 

6. To remove the milling bit, hold your finger underneath to prevent falling and breaking.

7. Screw on the right side of the red sticker to release mill.

![pcb](/assets/images/2022-05-13-pcb-milling/pcb3.jpeg)

![pcb](/assets/images/2022-05-13-pcb-milling/pcb1.jpeg) 

8. Push the new mill in almost all the way. Tighten the screw snugly but not violently.

9. Move the mill head all the way down and then slightly up.

We set the x- and y-origin using Mods. Before we can communicate between the Modella and Mods, we first have to set up communication.

##### Mods

###### Setting up communication

1. Open Mods by opening the terminal and type Mods. When a pop-up asks for authorization (2x) - just click cancel. A browser with Mods will open.

2. Open the right program by right click - Program - Open Program - MDX Mill - PCB.

3. Make sure the Modella is turned on. The machine will move to its "Home". 

4. Close all sockets & ports (Websocket serial and pyserial)

![pcb](/assets/images/2022-05-13-pcb-milling/pcb4.jpeg) 

5. Open the serial socket & port.

###### Setting the origin

1. Don’t use manual move to move the mill on the x and y-axis, because it will move back to origin when it starts. Only move by coordinates in Mods.

![pcb](/assets/images/2022-05-13-pcb-milling/pcb5.jpeg) 

2. Z leveling (do this AFTER setting the x- and y-axis): Level the mill on the surface (careful, don’t drop it, use your finger). Open the screw and let the mill rest on the copper plate. Secure the screw. 

If you move the origin, you also have to redo the Z axis alignment. This is because the bed is not perfectly level on every spot.

3. Make photo of origin - in case there is a power blackout or sorts.

##### Setting up the file

###### Milling the traces

1. You'll need a SVG or PNG file. Select your SVG or PNG file. Start with the file for the traces.

![pcb](/assets/images/2022-05-13-pcb-milling/pcb6.jpeg) 

4. For PNG's - set the DPI to 1000 - check the size of the image. 

5. Set pcb defaults - there are two screens to do this. Use the right one for metric system.

![pcb](/assets/images/2022-05-13-pcb-milling/pcb7.jpeg) 

6. Click mill traces.

7. Go to the window "mill raster 2D" 

![pcb](/assets/images/2022-05-13-pcb-milling/pcb8.jpeg) 

8. Set tool diameter to 0.4 mm (left side).

9. Set cut depth an max depth to 0.002 inch (!! right side, around 0.0508mm)

10. Set offsetnumber (amount of lines per trace) to 4.

11. Set Offset stepover to 0.5 (how much the lines overlap).

12. Press calculate. Black is going to be milled away. White are traces.

13. View your file - does it look like expected? - when it looks funky: troubleshoot. Make sure you have some black around the traces. 

14. Go to job settings (window where we also set the origin).

15. Set the cut speed on 4mm/s for 0.4 mm 1 flute (check for 2 flute, prob slower).

16. When you are ready - send file (Websocket serial window). The milling will start right away.

17. Vacuum the plate when the milling is done. 

###### Milling the outline

1. Replace the milling bit, we want to use the 0.8mm mill now. 

2. Move the mill to the origin and level the z-axis.

3. Select your SVG or PNG file for the outline. 

4. For PNG's - set the DPI to 1000 - check the size of the image. 

5. Set pcb defaults - Use the right one for metric system.

6. Click mill outline.

7. Go to the window "mill raster 2D" 

8. Set tool diameter to 0.8 mm (left side).

9. Set cut depth to 0.6mm and max depth to the thickness of the copper plate (1.55mm for example)

10. Leave offsetnumber (amount of lines per trace) on 4.

11. Leave Offset stepover on 0.5.

12. Press calculate.

13. View your file - does it look like expected? - when it looks funky: troubleshoot. Make sure you have some black around the outline. 

14. Go to job settings.

15. Leave the cut speed on 4mm/s.

16. When you are ready - send file (Websocket serial window). The milling will start right away.

17. Vacuum the plate when the milling is done. 

##### Notes

1. When you want to send a new file after interrupting the last session, first delete memory/reset machine by pressing up & down simultaneously for 10 sec.

2. If you want to pause the milling and see how its going, you can press "view" on the Modella. When you want to continue, press "view" again. 

##### Issues

1. The milling doesn't go all the way trough. - The plate is not level. Level the z-axis again or set the cut depth a little higher

2. The paths of the image are not closed after calculating. - Change the threshold. In my case 0.9 was not enough, but 0.95 did the trick.

##### How to make my own programmer

Michelle showed us how to make the SWD programmer, and i tried to solder all the components. I had a hard time soldering, since i've never soldered so small. During soldering i damaged the trace where the pin header was connected. I got very frustrated and decided to make my own board the next day.

##### Making the FTDI programmer

Henk sended me [this](https://gitlab.fabcloud.org/pub/programmers/programmer-serial-d11c) link to make the FTDI programmer. The milling went smooth. But then it was time to solder...

Jonathan showed me some techniques on how he solders and i watched [the fabacademy electronics production class](https://vimeo.com/678487187) where Neil also shows some tips & tricks on soldering. With this knowledge i went in again.. And made a nicely soldered board!! 

(picture of FTDI)

Henk tried to program the board, and although this was possible, we ran into an error. We tried to find the problem, but couldn't. I tested all the connections with a voltage meter and eventually discovered that one if the pins of the µC didn't connect. I applied more solder but this didn't help. The trace ran underneath the µC, so i had to de-solder the µC. I used a hot-air-gun for this. Underneath the µC the trace was a micro bit damaged. I fixed this with a little solder, and re-soldered the µC. All the connections worked fine after this and the PCB was programmable without errors.

Next i made the bridge to program other boards with my programmer. 

(picture Bridge)

And then only the broken SWD was left. I didn't want to give up on it, so i used a tiny red wire to fix the traces. Also the pinheader got loose in my bag and ripped some of the traces of the board. This i fixed with hot glue. I tested all the connections, next i want to be able to program the SWD myself

(picture SWD)

