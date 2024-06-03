---
title: "Hello World board"
---

In the previous week i've learned to mill and stuff a PCB, and how to program a PCB. Next i'll make my own Hello World board. For this i have to learn how to work with Kicad. I also have to figure out which components to use and which value the components need to have. I already have the Fab lib for Kicad installed.

I'll use this schematic supplied by Henk as a base and build on top of that:

![hello](/assets/images/2022-05-23-hello-world/hello01.png)

##### Components/BOM

The schematic contains the following components:

- ATtiny412
- Button_B35N
- LED 
- Capasitor (value unknown)
- Phototransistor_PT15-21C-TR8
- Resistor (value unknown)
- Pinheader FTDI 1x06
- Pinheader UPDI 1x03

I noticed that there is no voltage regulator in the schematic, Ben told me this is because the ATtiny runs on 5v. (The SAMD's run on 3.3v)

##### Eeschema

I placed all the components in Eeschema.

![hello](/assets/images/2022-05-23-hello-world/hello02.png)

Then the difficult part begun. I connected the pins i knew how to connect. 

![hello](/assets/images/2022-05-23-hello-world/hello03.png)

For the rest of the connections went to [Erwin's site](https://fabacademy.org/2021/labs/waag/students/kooi-erwin/report/week_06/) and looked at his schematic:

![hello](/assets/images/2022-05-23-hello-world/hello04.png)

The following questions arose:

- Why is there a resistor connected to 5v and pin 5 before the phototransistor?

- How do i know which pins from the µC to connect to my components?

##### Why is there a resistor?

Since the photoresistor is operating like a switch we need to use a pull up or a pull down resistor. 

As a comment in [this example](https://forum.arduino.cc/t/why-do-i-need-a-pull-down-resistor-in-the-button-example/364688) explains: 

"If there were no link between pin and 0V (GND here), the input would be floating when button is not pressed, because of the very high input impedance. The resistor gives this link needed to make sure the input is at 0V in this case. A wire would not allow the pin to be HIGH when button pressed, as it would make a short between 5V and 0V"

##### But why isn't there a resistor connected to 5v and pin 7 before the switch??

But since we saw in the last answer that we need a pull up resistor before a button to prevent the input from floating, why isn't there one before the switch on pin 7?

Again Ben to the rescue.

Ben told me that you can program the internal pull up resistor inside the µC instead of using a separate resistor. I knew arduino could do this. 

![hello](/assets/images/2022-05-23-hello-world/hello07.jpg)

The µC (in this example for the arduino, but also the ATtiny i'm using) has a lot of peripherals we can't see. We can use the internal resistor as a pull up resistor by initializing the pin where the button is connected (lets say pin2) as pinMode(2,INPUT_PULLUP);

But why do we do this for the switch and not for the photoresistor? 

Our guess is that this is because the resistance of the internal resistor is not so high, and fixed. This is fine for the button, but with the photoresistor we might need a higher resistance, and we want to be able to change this value. 

##### How do i know which pins to connect

Data sheeeet!!! 

![hello](/assets/images/2022-05-23-hello-world/hello06.jpg)

![hello](/assets/images/2022-05-23-hello-world/hello05.jpg)

But what does it all mean?

Name | Function 
--- | --- 
VCC | Power input (ATtiny412 can operate 5v)
GND | Power output
OUT (0 OUT, 1 OUT) | Logic output pin
IN (0 IN0, 0 IN1) | Logic input pin
MOSI | SPI; Master-out-slave-in
MISO | SPI; Master-in-slave-out
SCK | SPI; Serial Clock, the line that carries the clock signal.
DAC | Digital to Analog converter (DAC), used for converting digital pulses to analog signals
TXD | Serial; Transmit data (connect to RXD of other device)
RXD | Serial; Receive data (connect to TXD of other device)
SDA | I2C; The line for the master and slave to send and receive data.
SCL | I2C; Serial Clock, the line that carries the clock signal.
UPDI | Pin for UPDI, interface for external programming and on-chip debugging of µC's
CLK I | Clock In
GPIO (PA1, PA2, etc) | General Purpose Input/Output. PA1 corresponds to Pin 2 when coding)
CCL | Configurable Custom Logic

Other important things:

Spec | ATtiny412
--- | ---
Flash memory | 4096 bytes

##### Back to Kicad

Now i should be able to connect the rest of my pins. 

![hello](/assets/images/2022-05-23-hello-world/hello08.png)

I used labels for the Vcc and GND to make the schematic more tidy. 

To determine the value of the capacitor i looked at the ATtiny412 board examples on the [Fablab embedded programming page](http://academy.cba.mit.edu/classes/embedded_programming/t412/hello.t412.3.blink.png) and choose a value of 1µF.

The resistor before the LED will get a 330 Ω value. This is because the LED i'll use is a red LED. This requires 20mA and 1,8v

Voltage drop = 5v - 1,8v = 3,2v

U = I x R dus R = U/I

R = 3,2 / 0.02 = 160Ω

The closest thing we have in the lab that is or is higher than 160Ω is 330Ω.

The pullup resistor of the button will get a 10kΩ resistor based on [this guide](https://www.build-electronic-circuits.com/pull-up-resistor/).

For the phototransistor i didn't know how to choose the value, but both my Arduino Starters book and Erwin's example use a 10kΩ

Next i annotated the components and then assigned the footprints. 

![hello](/assets/images/2022-05-23-hello-world/hello09.png) 

Most of the footprints were already associated, i only had to assign the size for the LED, C and R. The size we use for SMD in the lab is 1206.

##### PCB Editor

Time to open up the PCB editor.

Henk told me to make my traces thicker. I should also put a clearance around my traces

![hello](/assets/images/2022-05-23-hello-world/hello10.png) 

Than a new question came up. I connected the capasitor to my µC, and Kicad showed my that i can connect the Vcc an GND of the connector to the capasitor. This will give me a figure 8, and i wondered if this will mess up my circuit, since the current doesn't have to travel through the µC to go from Vcc to GND, it can go through the cap instead. I didn't see this in other examples. I asked Henk and he told me it didn't matter. After searching for a little bit i found another example where the current can flow through the capasitor only.

![hello](/assets/images/2022-05-23-hello-world/hello11.png) 
![hello](/assets/images/2022-05-23-hello-world/hello12.png) 
![hello](/assets/images/2022-05-23-hello-world/hello13.png)
![hello](/assets/images/2022-05-23-hello-world/hello14.png)  

1st pic: figure 8, current can flow through cap only.

2nd pic: current needs to travel through µC.

3rd pic: hello board from Neil, current also needs to travel through µC.

4th pic: hello board from Neil, current can flow through cap only.

After a lot of struggling i managed to connect all the components without having to add a 0Ω resistor. 

![hello](/assets/images/2022-05-23-hello-world/hello15.jpeg) 

![hello](/assets/images/2022-05-23-hello-world/hello17.jpeg) 

I left a little blank space to see if i can place a logo. I also ran the DRC control and had some warnings about lines not being properly connected. I fixed the lines and ended up with a bunch of silkscreen errors, but since i'm not using silkscreen i ignored those.

![hello](/assets/images/2022-05-23-hello-world/hello18.jpeg) 

I opened the exported SVG in Photoshop, inverted the colors and added my signature. i couldn't export to SVG so i exported to PNG. But when opening the file in mods i ran into a problem. The DPI was set on 300dpi, mods wants files of 1000dpi. When i change the DPI the size of the board also changes. 

![hello](/assets/images/2022-05-23-hello-world/hello19.jpeg) 

Luckily i quickly found a way to change this in Photoshop. By editing the image size -> resolution on 1000 dpi/ppi (photoshop uses ppi) with resample checked i managed to set the correct dpi. 

![hello](/assets/images/2022-05-23-hello-world/hello16.jpeg) 

##### Milling the board

I tweaked the settings in Mods a bit, because i used a 2 flute mill. I set the speed on 3mm/s instead of 4, the depth per layer to 0.0015 instead of 0.0025 (Bas told me he did this to avoid the mill from breaking) and the threshold on 0.9.

The board turned out nice, some of the traces where a little thin, but they worked fine.

![hello](/assets/images/2022-05-23-hello-world/hello20.jpeg) 

##### Stuffing 

Next i collected all the components and stuffed the board. I had to use a 470Ω resistor because we had no 330 left. 

When testing the board with Bas, we noticed that i connected the traces of the UPDI connector wrong. This resulted in a smoking µC. I used the heatgun to remove the µC and placed a new one. I checked the traces with a voltage meter. The board didn't work. After some frustrating debugging i noticed i turned the µC upside down. One heatgun session later this was fixed. 

![hello](/assets/images/2022-05-23-hello-world/hello22.jpeg) 

##### Programming my Hello World board

This is the part where a lot of headache was caused. My computer didn't recognize my programmers. I tried a lot of USB connectors, different programmers, rebooted several times, but nothing worked. My boards were recognized by other computers. I thought it might had something to do with the M1 chip of my mac. But Bente and Ben both also have a M1 chip. Then i thought it had to do with the most recent security update, on 16-04. When i programmed my programmer it was recognized by my laptop, that was before the update. I couldn't find anything about this on internet, and no-one in the Fab mattermost said they had the same problem. 

I asked Michelle if she could see my hello world board and if she could program the board. She could see the board but programming was not possible. We tried different settings and board managers in Arduino IDE. 

But then Bente let me try her USB hub and programmer and it worked!! ON MY LAPTOP. Then also my USB hub worked with Bente's programmer. When i tried my FTDI programmer my computer stopped recognizing everything in my USB hub (this is a usb 3 hub, Bente had a usb 2 hub). Then when connecting Bente's hub and programmer, and then my hub and her programmer, my computer was able to recognize the boards again. 

This is problably caused by a short circuit because of my FTDI programmer, and some security protocol in my mac to avoid harming my usb connectors. 

I was able to upload a blink program to my board, but the led didn't blink. Then i noticed that i mixed up the anode/cathode side. A quick heatgun session fixed this.

Now i'm able to program my hello world. I heard no one in the fablab got the FTDI programmer version i have working. For now i use Bente her board but as soon as the mill is free i'll make a dual serial programmer. 

![hello](/assets/images/2022-05-23-hello-world/hello21.jpeg) 