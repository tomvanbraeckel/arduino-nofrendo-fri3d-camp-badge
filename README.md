# arduino-nofrendo for Fri3D Camp Badge

This is a special version of the nofrendo NES emulator, ported to work an an Arduino library.

Credit: https://github.com/moononournation/arduino-nofrendo.git

Then it was ported to the [Fri3D Camp 2022 Badge](https://fri3d.be/badge/), and hopefully soon to the Fri3D Camp 2024 badge.

# Demonstrations

Click the images for a video demonstration:

[![SuperMario1](/images/IMG_20240608_112621404.jpg)](https://www.youtube.com/watch?v=mjQrPMMnwLo)
[![SuperMario2](/images/IMG_20240608_112636094.jpg)](https://www.youtube.com/watch?v=mjQrPMMnwLo)
[![PacMan1](/images/IMG_20240608_115050745.jpg)](https://www.youtube.com/watch?v=A5guqi13I2M)
[![PacMan2](/images/IMG_20240608_115120776.jpg)](https://www.youtube.com/watch?v=A5guqi13I2M)

## What works

### Fri3D Camp 2022 Badge:

Done:

* Display works
* Audio works using the buzzer, so quality is "not great, not terrible"
* ROM files are read from SPIFFS filesystem (as it doesn't have an SD card reader)
* START button (connect GND to IO13, the "2" pad) works

TODO:

* More button configuration (the badge doesn't have buttons)

### Fri3D Camp 2024 Badge:

* All TODO (I need a prototype board!)


## Installation

- Install the Arduino IDE (tested with v1.8.13 but newer might also work)
- Install esp32 (tested with v2.0.17) using Tools - Board - Boards Manager.

- Enable the "ESP32 Dev Module" from Tools - Board:... - ESP32 Arduino - ESP32 Dev Module
- The default settings should be okay:
	- Upload Speed: 921600
	- 240Mhz
	- 80Mhz
	- QIO
	- 4MB (32Mb)
	- Default 4MB with spiffs
	- Debug level: verbose
	- PSRAM: disabled
	- Arduino runs on: core1
	- Events run on: core1
	- Port: /dev/ttyUSB...

- Download this arduino-nofrendo-fri3d-camp-badge library into ~/Arduino/libraries and restart the Arduino IDE.
- Install GFX_Library_for_Arduino using the Arduino Library Manager or download it manually from https://github.com/moononournation/Arduino_GFX into ~/Arduino/libraries

- To upload files into the ROM (SPIFFS) of the Fri3D Camp 2022 Badge (which has no SD card reader), install "ESP32 Sketch Data Upload" into ~/Arduino/tools/
Then restart the Arduino IDE, ensure the Arduino Serial Monitor window is closed and run "Tools" - "ESP32 Sketch Data Upload".
This should upload the files examples/esp32-nofrendo/data/ into the SPIFFS.
If it doesn't work, you might need to format it first, see "Formatting SPIFFS" below.

- Open the code using: File - Examples - Examples from Custom Libraries - arduino-nofrendo - esp32-nofrendo
- Execute the code with Sketch - Upload

- Now the device should boot, load the first .nes file from the SPIFFS (Chase.nes) and start it.


# Usage

This library comes with a custom example "Chase.nes" game, see examples/esp32-nofrendo/data/Chase.txt for details.

The buttons haven't been configured properly yet, except the "START" button.
So you should be able to start a game by connecting the "2" pad (GPIO13) to GND to press "START".


# Formatting SPIFFS

To manually format the SPIFFS filesystem, put this below:

```
display_begin();
```

in esp32-nofrendo.ino:

```
Serial.println("Formatting SPIFFS, this can take a minute...");
bool formatted = SPIFFS.format();
if(formatted){
    Serial.println("\n\nSuccess formatting");
}else{
    Serial.println("\n\nError formatting");
}
```

Then upload the code. It should format the SPIFFS.
Don't forget to remove or comment out the formatting code after that and reupload the sketch.

After formatting, you can upload the files into the SPIFFS using "ESP32 Sketch Data Upload" as described above.
