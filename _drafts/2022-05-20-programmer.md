---
title: "Programming a programmer"
---

#### How to program a programmer

So first i have to say i'm diving into a rabbit-hole by wanting to figure out how to program a programmer myself. 

I found [this site from MTM](https://mtm.cba.mit.edu/2021/2021-10_microcontroller-primer/) explaining a lot about µC's AND how to program them by making a programmer. 

It also gave me a little clarity on some acronyms flying by in the past few days.

JTAC (joint test action group) - protocol that is standard for many µC's (ARM cores)

SWD (serial wire debug) - 2-pin variant of JTAC. Most common on newer ARM chips

CMSIS-DAP - programming device firmware. Runs on the programming device (the programmers we're making). Communicates over USB to the PC and on the other side over TTL (simple physical pins) to the µC

CMSIS-DAP device - programmer. Any device that can write programs into the µC's memory using JTAC or SWD

[Bootloader](https://learn.adafruit.com/bootloader-basics/) - a "programming program" that runs when powering a µC to check if there's a new program to be loaded and loads it if needed (to load the code we wrote in its memory).

To program a programmer we need [this binary file](http://academy.cba.mit.edu/classes/embedded_programming/SWD/free_dap_d11c_mini.bin) and an other CMSIS-DAP device

#### Getting all the requirements 

**So we need a CMSIS-DAP tool - a programmed programmer.**

The CMSIS-DAP tool should be connected to the PC by USB

The CMSIS-DAP tool should be connected to the programmer i want to program by the SWD pinout. Orientation is important, but by using the wrong orientation i wont hurt my programmer. 

My programmer connected to a powersupply (by using another USB port)

**And we need a CMSIS-DAP software**

I'm going for EDBG - Bas told me this is a simpeler program. Although is known not to work well woth OSX, but there's a way to bypass this. 

Installing EDBG (Mac M1):

// I got the latest build from [this repo](https://github.com/ataradov/edbg/actions)

1. Download the EDBG code from [this repo](https://github.com/ataradov/edbg/)

2. Install hidapi - i already have brew installed so i installed hidapi by opening the terminal and run: brew install hidapi

Then i saw that i missed a step. I didn't use Rosetta to open terminal before installing hidapi. 

So i went a step back and ran brew uninstall hidapi. 

I closed terminal and looked up the terminal app in my Finder window and then pressed right click - Get Info - Open using Rosetta.

FYI: Rosetta is an program to force an app to use the Intel based environment of a program instead of the M1 chip environment. 

(screenshot rosetta)

I opened the terminal again and ran brew install hidapi. 

Then i saw that Homebrew needed to be installed in /usr/local/, while my homebrew is installed in /opt/homebrew/. I decided to ignore this, and come back to this point if i run into errors. 

3. Open make file and change libhidapi.a to libhidapi.dylib : The makefile didn't have libhidapi.a, so i did not change the makefile

4. Run Make all : I navigated to the repo and ran make all in the terminal, resulting in the following error  
    
    make: pkg-config: Command not found
    make: pkg-config: Command not found
    gcc  -W -Wall -Wextra -O3 -std=gnu11 dap.c edbg.c target.c target_atmel_cm0p.c target_atmel_cm3.c target_atmel_cm4.c target_atmel_cm7.c target_atmel_cm4v2.c target_mchp_cm23.c target_st_stm32g0.c target_gd_gd32f4xx.c target_nu_m480.c target_lattice_lcmxo2.c  dbg_mac.c -framework IOKit -framework Foundation -framework CoreFoundation -framework Cocoa  -o edbg
    dbg_mac.c:36:10: fatal error: 'hidapi.h' file not found
    #include "hidapi.h"
         ^~~~~~~~~~
    1 error generated.
    make: *** [edbg] Error 1

I checked if hidapi was really installed with "brew list" and noticed hidapi was not there. I ran "brew install hidapi" again and saw the following error:

        Error: Cannot install under Rosetta 2 in ARM default prefix (/opt/homebrew)!
    To rerun under ARM use:
        arch -arm64 brew install ...
    To install under x86_64, install Homebrew into /usr/local.

Lets take another step back.

Installing homebrew for Intel apps:

Before i can install hidapi i need to install homebrew for intel apps, meaning installing homebrew in /usr/local instead of /opt/homebrew/

To do:

Install .tar homebrew in /usr/local
Set terminal to use rosetta to behave as an Intel chip program
Install hidapi
Edit make file and run make
Install EDBG

Then Henk told me i don't need to use EDBG. I can use the arduino IDE instead. 

But when Henk wasn't there i couldn't figure out how to, so i continued installing EDBG.

1. Install Rosetta 2 by using this command: softwareupdate --install-rosetta

2. Open terminal as an Intel chip program

2. Install Homebrew for Intel chip programs: see [this tutorial](https://medium.com/mkdir-awesome/how-to-install-x86-64-homebrew-packages-on-apple-m1-macbook-54ba295230f)

I made an alias in my zshrc file. Now i can install homebrew software for Intel by using ibrew instead of brew

3. Run ibrew install hidapi

4. Clone the GIT repo 

5. Edit the make file: Because i didn't know this step i got a lot of errors that when i tried "make all" the hidapi libs could not be found. I looked at previous makefiles in the repo and saw that the following lines were deleted:

LIBS += /usr/local/lib/libhidapi.a
CFLAGS += -I/usr/local/include/hidapi

i added them and changed them to the location of my hidapi libs:

LIBS += /usr/local/homebrew/lib/libhidapi.dylib
CFLAGS += -I/usr/local/homebrew/include/hidapi

I also edited the dbg_win.c file due to [Matti's documentation](https://fabacademy.org/2022/labs/aalto/students/matti-niinimaki/weekly-assignments/week08/):


Find the line that says static int report_size = 512;
Change it to static int report_size = 64;

6. Run Make all (from the edbg folder) to install EDBG

7. EDBG is finally installed and working!

#### Programming SAMD11 circuits with Arduino IDE

**Circuit requirements**

To make circuits that are programmable with the Arduino IDE the circuits needs a few basics (as copied from the [MTM site](https://mtm.cba.mit.edu/2021/2021-10_microcontroller-primer/fab-arduino/#1-schematic-requirements)):

1. Power and communication to the board (i.e. a USB hookup)

2. Voltage regulation (USB gives 5v/500ma, our µC needs +3v3 - we need a regulator to convert the voltage)

3. Bypass capacitors (these help stabilize voltage inputs to the µC)

4. a debug / programming port (we need this in order to load the bootloader)

**Installing the bootloader**

To use the Arduino bootloader on our SAMD11 we first need to program that bootloader into the µC using a programmer.

For this (this is specifically for ATSAMD11 & ATSAMD21 series) we use a CMSIS-DAP tool and edbg/pyOCD or OpenOCD to load the bootloader binary onto the board. The binary is specific to the µC we're using. 

[this](https://github.com/qbolsee/ArduinoCore-fab-sam/blob/master/bootloaders/zero/binaries/sam_ba_SAMD11C14A.bin) is the binary for the ATSAMD11C.

**Arduino setup**

To make our boards compatible with the Arduino IDE we need to follow the next steps in the Arduino IDE.

1. Go to file -> preferences -> additional board manager URLS

2. add this link: https://raw.githubusercontent.com/qbolsee/ArduinoCore-fab-sam/master/json/package_Fab_SAM_index.json
now arduino will be able to find the “board definitions” when we do:

3. tools -> board -> board manager
search for Fab SAM core for Arduino
hit install

Now we can use the Arduino IDE to write code for our fab boards.


