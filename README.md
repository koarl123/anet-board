# Introduction
This project provides a board definition which enables the Arduino IDE to compile firmware (such as Marlin) for Anet 3D printers.  It is used in the SkyNet3D Marlin distribution for Anet printers, but can be used independently to, for example, build Marlin from source.

## Installation Instructions
Copy the included 'anet' folder into the 'hardware' folder of your Arduino installation.

1. Download the most recent version from the 1.8.x branch of the Arduino IDE from 
   https://www.arduino.cc/en/main/software
2. Download a zip file of the master branch of this project using the 'Clone or Download' button
3. Install the Arduino IDE using the default options

Then:

#### Windows
4. Open your `Documents` folder in Windows Explorer
5. Open the `Arduino` folder, then the `hardware` sub-folder.  If neither of these folders exist, create them.
6. Open the anet-board zip file you downloaded in step 2 and copy the 'anet' folder into `Documents\Arduino\hardware`

#### OS X
4. Click on "Documents" in Finder and look for the `Arduino` directory inside it. The full path is `~/Documents/Arduino/hardware`
5. Open the anet-board zip file you downloaded in step 2 and copy the 'anet' folder into `~/Documents/Arduino/hardware`

## Using the Board Definition
1. Open the Arduino IDE
2. Open the Tools->Board menu and select either of the Anet V1.0 boards
3. Open the Tools->Port menu and select the appropiate COM port (the one which appeared when you plugged your printer into a USB port)

**Note:** The Optiboot board option is highly recommended as the Optiboot bootloader uses much less space than the standard Atmega 1284p bootloader, allowing more flash space for the firmware.  If you choose to use the Optiboot option you **MUST** burn the Optiboot bootloader before uploading firmware, otherwise you risk overwriting the bootloader.

When uploading firmware, use Sketch->Upload (Ctrl+U), which uploads over the USB connection, **not** Sketch->Upload Using Programmer (unless you're actually using an Arduino programmer and know what you're doing).
## Check connection hints on page 
https://www.marcuslausch.de/2017/03/10/ctc-prusa-i3-bootloader-installieren/
Connection is done via J3 and power supply is provided by the USBasp via 5V VCC at LCD plug!!

## Prerequisites
Backup the previously flashed code via avrdude using the command
> avrdude avrdude.conf -c usbasp -b 57600 -i 10 -p m1284p -U flash:r:factory_Anet10_FLASH.hex:i

Then, store EEPROM data the same way:
> avrdude avrdude.conf -c usbasp -b 57600 -i 10 -p m1284p -U eeprom:r:factory_Anet10_EEPROM.hex:i

Do the same with fuses and locks:
> avrdude avrdude.conf -c usbasp -b 57600 -i 10 -p m1284p -U lfuse:r:factory_Anet10_LFUSE.hex:i

>avrdude avrdude.conf -c usbasp -b 57600 -i 10 -p m1284p -U hfuse:r:factory_Anet10_HFUSE.hex:i

> avrdude avrdude.conf -c usbasp -b 57600 -i 10 -p m1284p -U efuse:r:factory_Anet10_EFUSE.hex:i
## Burning the Bootloader
Burning the bootloader to the board requires either an Arduino programmed as an ISP, or a USBasp.  The programmer is connected to the middle six pins of the J3 connector on the Anet v1.0 board.

Burning the bootloader to an Arduino always erases the flash memory.  Once the bootloader has been burned using a programmer the programmer should be disconnected and firmware uploaded using the serial-over-USB connection.

With the programmer connected:
1. Launch the Arduino IDE
2. Select the appropriate board from the Tools->Boards menu - either `Anet V1.0` or `Anet V1.0 (Optiboot)`
3. Click Tools -> Burn Bootloader
4. The board will reset when complete, and the LCD display will be blank

Assuming there were no errors, at this point the firmware has been erased and will need to be re-uploaded.  The programmer should be disconnected.
