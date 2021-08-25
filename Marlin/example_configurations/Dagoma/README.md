# Marlin for Dagoma Printers

Based on the initial firmware for the Discovery 200, available at https://dist.dagoma3d.com/ using the following options:

- I choose the platform that fits me: Discovery 200
- Did I add one or more options to my printer? No
- What is the type of my endstops? NC
- What is my printhead ? Easy or standard? Standard printhead
- What is the color of my probe? White

## Build

Compiled successfully using Arduino IDE 1.8.15
Uploaded using Dagom'App available at https://dist.dagoma3d.com/

### Arduino IDE Configuration for Discovery 200

- Download whether portable or install version of Arduino IDE
- Run Arduino IDE
- Go to File > Preferences
- In the dialog, copy to "Additional Boards Manager URLs" the following: https://raw.githubusercontent.com/Lauszus/Sanguino/master/package_lauszus_sanguino_index.json
- Then go to Tools > Board: "XYZ" > Boards Manager...
- Type "Sanguino" in the search box, and install the latest version (1.0.3 at time of writing)
- You can now seelct the Sanguino board from Tools > Board: "XYZ" > Sanguio-avr > Sanguino
- Then select Tools > Processor: "XYZ" > ATmega1284 or ATmega1284P (16Mhz)
- In order to use an LCD screen, you'll need to go to Sketch > Include Libray > Manage Libraires...
- From here, type "u8glib" in the search box and install the latest version of (1.19.1 at time of writing)

In order to upload the firmware to the Dagoma board without error using the IDE, you should adjust some more parameters:

- Open C:\Users\%%USERNAME%%\AppData\Local\Arduino15\packages\Sanguino\hardware\avr\1.0.3\borads.txt
- Search for "sanguino.menu.cpu.atmega1284p=ATmega1284 or ATmega1284P (16 MHz)"
- From here change sanguino.menu.cpu.atmega1284p.upload.speed=115200 to 57600

### Optimizing Binary

You can optimize the compiled binary and save some kilobytes modifiying the compilation options.

In your Arduino IDE installation folder, go to `\hardware\arduino\avr` and add a new file called `platform.local.txt`
with the following content:

```
compiler.c.extra_flags=-fno-tree-scev-cprop -fno-split-wide-types -Wl,--relax -mcall-prologues -finline-limit=3
compiler.cpp.extra_flags=-fno-tree-scev-cprop -fno-split-wide-types -Wl,--relax -mcall-prologues -finline-limit=3
compiler.c.elf.extra_flags=-Wl,--relax
```

You can also add `-ffast-math` to save around 1.2k more.

Source: https://thborges.github.io/blog/marlin/2019/01/07/reducing-marlin-binary-size.html