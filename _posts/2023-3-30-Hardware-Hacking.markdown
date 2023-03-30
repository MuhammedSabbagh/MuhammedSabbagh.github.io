---
layout: post
title:  "Hacking the Sonoff R2 Basic (And Basicly all Sonoffs)"
date:   2023-02-22 14:25:27 +0300
tags: hardware hacking 
description: hardware hacking is The computer engineer's delight when things works exactly the way it shouldn't do!
categorize: Hardware 
img: assets/img/14.jpg
---

## Introduction

Recently, i went to the market to buy a microcontroller, to be honest i wasn't really certain on what to buy, however i kept in mind it needs to have a wifi chip or at least a modern form of connection to help doing Internet Things, and what else better than an ESP! so i bought it and came back home and started fiddling with it, at first i have practiced flashing erasing dumping the memory until i feeled like i am good at this point, I then installed the Micropython fiddled with it too; more on that later.

## Sonoff's unbelievably hackable products

few months ago my father told me about someone who sells smart home products, IoT, Power Regulators Amps etc and told me to buy a product that allegedly switch power on and off over wifi i bought it and brought it back home, and sure enough, it did just about that, after some IoT delight i left it for time to come.

few days later i decieded to open it up and found out it has esp chip (this is still from months ago back when the idea of having an ESP didn't cross my mind yet) i was like nice we might get some inspiration from this module and putted it back on, honestly it didn't cross my mind that this thing has pretty much open memory to dump and load.

fast forward and few days ago i decieded that i wanna see what can i do with it some googling later and found out about [this](https://templates.blakadder.com/sonoff_BASICR2.html), this article gave me a clue about the hacking process and the do\don't that comes with it, if you are interested about this you can have a look but the fun didn't end here.

## The Journey Begins

for having loads of free times means doing something useful with it, i soldered the UART pins on the Sonoff and then connected it to my Raspberry Pi, Why? because my CH340G was toasted by a ground short rendering it useless,then setting the serial up on the RPi as a Serial Port Not the Pi's terminal, I also installed esptool.py to flash and dump the firmware from that device, here, all the tools are ready now time for the real deal, to start keep this in mind:

{% include figure.html path="assets/img/14.jpg" title="Picture of Sonoff R2 Basic connected to Raspberry Pi using UART" class="img-fluid rounded z-depth-1" %}

- The MCU on the Sonoff is esp8285 which has internal 1MB of flash Memory working on DOUT mode, this isn't much of a concern to us really as the only thing we want to do is to dump and load firmwares, Also 1MB is more than Enough for multitude of things we can think off (RAM-like esp8266-is small tho at 112KB only!).

- The MCU Works in different modes and not touching the switch will put it into Operation Mode

- The Logic Voltage of the MCU is 3.3V DON'T UNDER ANY CIRCUMSTANCES POWER THE MCU WITH 5V!!! IT COULD EASILY KILL THE MCU!

- The Switch has 220/110V (Depends on the region Sold for) HV circuitry HOWEVER IT SHOULD NEVER BE POWERED WITH 220V WHILE BEING EXPOSED, IT RISKS GETTING YOU SHOCKED, OR WORSE DAMAGE THE BOARD SO ONLY USE THE UART TO POWER THE MCU!

with that out of the way let's commence!

## Dumping the Firmware

to do so we must enter the flashing mode, remove the power from the board and hold the button while powering it again, it should never blink once, if this happend; then you are in flash mode, if it blinked or LED turned on and off and heard the relay tick then you didn't enter flashing mode.

afterward we can excute this command to let us dump the flash memory.

```bash
:~ $ esptool.py read_flash 0x00000 0x100000 fwbackup.bin    
```

Normally if things went alright you should see this output going:

```txt
esptool.py v4.5.1
Found 1 serial ports
Serial port /dev/ttyAMA0
Connecting.....
Detecting chip type... Unsupported detection protocol, switching and trying again...
Connecting...
Detecting chip type... ESP8266
Chip is ESP8285N08
Features: WiFi, Embedded Flash
Crystal is 26MHz
MAC: 24:a1:60:19:aa:75
Uploading stub...
Running stub...
Stub running...
1048576 (100 %)
1048576 (100 %)
Read 1048576 bytes at 0x00000000 in 96.1 seconds (87.3 kbit/s)...
Hard resetting via RTS pin...
```

## Installing Micropython Firmware

This is so cool! but now here is the cooler Part! How about we install micropython on this thing and write our own code! say i want to make the same board turn the relay on and off with a the click of a button here is how to do it.

first let's go back to our friend [here](https://templates.blakadder.com/sonoff_BASICR2.html) here he explained what every GPIO on the MCU is used for but our attention for this right now will only go to GPIO00 (BUTTON) and GPIO12 (RELAY), so basically one will do output and one will do input.

with that said head over to [ESP8266 MicroPython Download's Page](http://micropython.org/download#esp8266) as 8285 is pretty much 8266 with internal flash memory and download the latest stable build.

to flash the firmware power cycle the board while holding the BUTTON and it will not blink indicating it's on programming mode then enter the following command.

```bash
 :~ $ esptool.py --port PORTNAME erase_flash
 :~ $ esptool.py --port PORTNAME --baud 115200 --chip auto write_flash 0x0 esp8266-1m-20220618-v1.19.1.bin -fs 1MB -fm dout
```

at the end you should See this :D

```txt
Configuring flash size...
Flash will be erased from 0x00000000 to 0x00090fff...
Flash params set to 0x0020
Compressed 591476 bytes to 392427...
Wrote 591476 bytes (392427 compressed) at 0x00000000 in 35.0 seconds (effective 135.2 kbit/s)...
Hash of data verified.

Leaving...
Hard resetting via RTS pin...   
```

cool now do a power cycle again for the last time hopefully and access the micropython interpreter using the Serial Terminal, you can use Putty, Screen, or Picom depending on your preference and access the board's UART with baud rate of 115200 bps like this for example

```bash
screen /dev/ttyAMA0 115200
```

you should see this everytime you powercycle of do a soft reset

```txt
MicroPython v1.19.1 on 2022-06-18; ESP module (1M) with ESP8266
Type "help()" for more information.
>>> 
```

now let's do a test and write a code to test if everything is alright

```python
from machine import Pin

# set up the GPIO pins

button = Pin(0, Pin.IN, Pin.PULL_UP)
relay = Pin(12, Pin.OUT)

while True:
    # check if the button is pressed
    if not button.value():
        # turn on the relay
        relay.value(1)
    else:
        # turn off the relay
        relay.value(0)
```

paste the code on the terminal and try pressing the button and the relay and its led should light!

## Conclusion

these are really fun devices to play with, we started from simple purposed device like an IoT Switch and ended up with programmable switch where we can do manything with, like imagine if i wanted that switch to start and stop at certain time, it would be awesome when it's needed, finally, i hope you learned something and it was a good time reading and peace!

---

### Special Thanks

- aaron: for being the First Supporter for the blog!

Consider donating [Here](https://theengineer.pages.dev/donate/) To Get Shout out like the one on top, Thanks!
