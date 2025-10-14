The badge, TiLDA, is being designed and built by Charles Yarnold and Tom Wyatt

## Getting wireless communication working

We use the Mirf library to achieve inter-badge communication. You can download Mirf from http://arduino.cc/playground/uploads/InterfacingWithHardware/Mirf.zip. However, the library has a flaw: it won't compile unless you add #include <arduino.h> to libraries/Mirf/Mirf.cpp.

The resulting lines 52-53 should look like this:

```cpp
#include <arduino.h>
#include "Mirf.h"
```

Also, the ce and csn pins must be changed to match TiLDA's design:

```cpp
  Mirf.cePin = A0;
  Mirf.csnPin = A1;
```

(you must add these lines to your `setup()` function).

## General overview

The badge must be a great introduction to micro-controllers, get people to interact more than the normally would and also be fun!

### Plan 1 (old)

The plan is to make an arduino based device that during the day encourages you to mingle with other sides of the camp, this will result in other badges being set to the same team as yours. At a set point of the evening the badges change mode into a camp wide came of laserquest, what team will survive?

### Plan 2 (new)

Zombie tag (infected i, humans h, healers he, zombies z) + Wireless Fingerprinting for zones + Heat maps + Domination / Changing the colour of a zone (on the RGB base stations by spending time there as either i, h, he, or z).

## BOM

https://docs.google.com/spreadsheet/ccc?key=0Ah0uj2i4tQJKdHVMM25ZRFpyd2JsUDl6VlNpVkFDb2c

## All code and datasheets

http://github.com/emfcamp/TiLDA

## What pin is X?

```cpp
enum Pins { PIN_LED_BOTH = -1,
            PIN_LED_RIGHT = 7,
            PIN_LED_LEFT = 4,
            PIN_LED_BLUE = 5,
            PIN_LED_GREEN = 6,
            PIN_LED_RED = 10,
            PIN_IR_IN = 11,
            PIN_IR_OUT = 3,
            PIN_BUTTON = 2
            //PIN_NRF24_IRQ = Does anyone know?
          };
```

NRF IRQ pin is not connected at all according to the schematic's on GitHub

## Software/Hardware Bugs/Fixes

### Bug 1

The default fuse values interfere with the operation of the wireless module, this can be fixed, provided you have access to an isp programmer by running:

```
sudo avrdude -c <PROGRAMMER> -p m32u4 -U hfuse:w:0xd9:m
```

where you replace <PROGRAMMER> with the name of the one you're using.

Note : the process for installing the successful bootloader described below results in hfuse being set to D8, not D9. D9 sets the reset vector to 0x0000, D8 sets it to the bootloader address. It's possible that d9 partly works, because if the device has been erased execution will eventually reach the bootloader but reprogramming later is likely to prevent this.

### Bug 2

Rx, Tx and pin 13 flashing quickly, device not responding. Need more info, for now, return badge and say it is faulty and try another one. We will look into this and get a patch out.

### Bug 3

Unspecified or other hardware bug. Check battery level. Power cycle the device and try to reflash the

!!! warning "NOTE"

    Unlike the Arduino Uno, the reset button needs to be pressed when uploading the sketch for it to upload properly.

    Think this is a bug in the way the boot loader and fuse have been done on all the boards, if a board has its bootloader uploaded using the burn boot loader function in the Arduino IDE, the Tilda will then correctly software reset when using uploading a new program. --'RepRap' Matt (talk) 19:29, 1 September 2012 (UTC)

### UPLOAD BUG: Re-burning the Boot loader

Bootloader needs to be refreshed with an ISP programmer for easy using the Arduino IDEs Burn bootloader function

These are the `avrdude` commands taken from the Arduino IDE doing a "Burn Bootloader"

```
 avrdude -C/Applications/Arduino 1.0.1.app/Contents/Resources/Java/hardware/tools/avr/etc/avrdude.conf -v -v -v -v -patmega32u4 -cusbasp -Pusb -e -Ulock:w:0x3F:m -Uefuse:w:0xcb:m -Uhfuse:w:0xd8:m -Ulfuse:w:0xff:m
 avrdude -C/Applications/Arduino 1.0.1.app/Contents/Resources/Java/hardware/tools/avr/etc/avrdude.conf -v -v -v -v -patmega32u4 -cusbasp -Pusb -Uflash:w:/Applications/Arduino 1.0.1.app/Contents/Resources/Java/hardware/arduino/bootloaders/caterina/Caterina-Leonardo.hex:i -Ulock:w:0x2F:m
```

## Coding the badge

You need to create a github account, verify your e-mail address and add an SSH pubkey (generate with ssh-keygen and copy the contents of ~/.ssh/id_rsa.pub) to your github account. This will prevent permissions errors when attempting to download the source from github. Use the following command to check out the repository:

```
 git clone --recursive https://github.com/emfcamp/TiLDA
```

To compile and program the board with the default firmware, you need to copy the "TiLDA/source" directory to a directory called "TiLDA", then open the "TiLDA.ino" file from within the Arduino IDE. From within Arduino select "Tools->Boards->Arduino Leonardo" and select the serial port corresponding to your device ("e.g. ttyACM0"). You can then compile and upload the sketch to the board, before pressing upload hit 'reset' on the board for programming to succeed. The default firmware will cycle the L.E.D's once each, and then do nothing, however if S1 is pressed during this cycle a "torch mode" is enabled. Try adding the following code to the "loop()" function to add a L.E.D light show:

```cpp
 led_cycle(&lights, PIN_LED_LEFT, 250);
 led_cycle(&lights, PIN_LED_RIGHT, 250);
```

You can now begin exploring the additional firmware and libraries. Copy the contents of the "libraries" directory to your "~/sketchbook" folder. A change is required to Mirf.cpp to use the Mirf library, add "include <arduino.h>" to the top line of Mirf.cpp. You can then begin exploring the library examples.

### Howto

1) Download and run Arduino 1.0.1
2) Select Tools -> Boards -> Arduino Leonardo.
3) Select Port -> ttyACM0 (normally) or ttyACM1 or ttyACM2
4) Just Program the board as normal with Examples -> Blink.
5) Note: the board does NOT auto reset when uploading from the Arduino IDE, please push the reset button just after the terminal at the bottom of the IDE says, e.g.:

```
"Binary sketch size: 4,858 bytes (of a 28,672 byte maximum)"
```

The code should then be uploaded as normal.
Active Development of Tilda Code

- We will be releasing some code asap / later this evening with an interactive game.
- https://github.com/emfcamp/TiLDA or https://github.com/emfcamp/TiLDA-source
- Join us on irc channel #emfcamp

### Todo

- Get a (small footprint) working ir lib for the ATmega32u4
- Test existing wireless code with ATmega32u4
- Write base game (this is happening now! join us on #emfcamp on IRC to help)

### Game

To get a working simple version to then build on I propose we:

Make the badges beacon out over ir their team and user id. Record the team ids you see in a rolling buffer. At predetermined intervals, say 5 seconds, each team id you have seen from different users equates to a score, with more users on one team earning less points i.e. 1 unique user on team red = 10 points, 5 unique users on team blue = 5 points. These get added to your current levels for those teams. If one of those levels reaches a determined threshold your badge gets converted to that team.

Should you not see a team id for a certain amount of polls, your counter for them decreases.

Seeing your own team decreases all team counters (but at a low rate as to not effect interchanges)

Your badge should beacon out over 2.4 messages about its colour, who it sees and when it gets converted.

## Contact

The plan is very fluid at the moment, once we have it more locked down we will have more details on the wiki, feel free to join us in #openlasertag on the freenode irc network

## Game mechanics

Everyone chooses a team.

During the day people switch teams.

As you see other people from other teams your counter for their team increases. If this level reaches a certain amount then you switch to their team.

Those levels decay at one rate if that team has not been seen, being with your own team decreases all other levels faster.

## Link Dump / Other Ideas for the badge

-  Alternate IR library: http://www.arcfn.com/2009/08/multi-protocol-infrared-remote-library.html (git hub: https://github.com/shirriff/Arduino-IRremote/)
  - Tested receiver code with DVD remote control and seems to work, not yet able to confirm working IR send code (possible board problem?) -- SamLR 27-08-2012 01:42

## Build Brighton IR Badge project

- Project Page on Build Brighton Wiki http://www.buildbrighton.com/wiki/Light_Brigade_IR_Badge_Board_and_Soldering_Workshop
- Github page for the badges https://github.com/mikepea/buildbrighton-ir-badge
- IR Badge Zombie Tag rules http://www.buildbrighton.com/images/1/18/BB_Badge_rules.png
- Code for a IR+RGB badge that can talk to other badges: https://github.com/mikepea/white-night-kit/
- Flickr / Badges http://www.flickr.com/photos/andrewsleigh/sets/72157626092949670/
