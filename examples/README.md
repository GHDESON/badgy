## Setup
- Install Arduino IDE https://www.arduino.cc/en/Main/Software
- Add ESP8266 Arduino Core using the Boards Manager https://github.com/esp8266/Arduino#installing-with-boards-manager
- Install the following libraries for the various examples
    - Adafruit GFX library https://github.com/adafruit/Adafruit-GFX-Library
    - GxEPD v3.x library https://github.com/ZinggJM/GxEPD
    - ArduinoJSON v5.x library https://github.com/bblanchon/ArduinoJson
    - WiFi Manager library https://github.com/tzapu/WiFiManager
    - Time v1.5 https://github.com/michaelmargolis/arduino_time
    - WebSockets https://github.com/Links2004/arduinoWebSockets
- When compiling the sketch in the Arduino IDE, choose **Tools** > **Board** > **NodeMCU 1.0 (ESP-12E Module)**    

## Uploading Your Own Firmware

### Over USB (For Rev 2A/2B)
1. Once the right board and port is selected, simply click upload and your sketch will be uploaded automatically. Make sure the power switch is on (down), and you should see the the blue LED on the ESP-12F module flsahing.

### Over Wifi (For Rev 1/2A/2B/2C)
1. In the Arduino IDE, go to *Sketch* -> *Export compiled Binary* to create your firmware binary `*.bin`
2. Slide the power switch off (up), hold the center button and slide the power switch on (down) at the same time. Your Badgy should now be in OTA mode.
3. Go to http://*BADGY_IP_ADDRESS*:8888/update to upload your new `*.bin` file
4. Badgy will now restart with your new firmware
### Note: If your new firmware does not include the WiFi and OTA code, you will no longer be able to update over WiFi!

## Converting Your Own Image
Badgy has a 296x128 black & white e-paper display, you can upload your own bitmap image.
1. Go to http://javl.github.io/image2cpp/ and upload your own image file
2. Resize the image so that it is within the dimensions of the display (works best if dimensions are multiples of 8 e.g. 72x64)
3. Select **Arduino code** under **Code output format** and click the **Generate code** button
4. Copy and paste the output into your code; you can create a new header file (e.g. myimage.h) or simply replace the ```hello.h``` file in the Hello example. Remember to include the file in your main sketch.
5. The method ```display.drawBitmap(image, 0, 0, 296, 128, GxEPD_WHITE);``` draws the bitmap on the screen. The parameters are in the following order: image byte array, x, y, width, height, and background color. Call ```display.update()``` after to draw the new image.

## Restoring To Default Firmware
1. Download the default firmware [badgy.bin](https://github.com/sqfmi/badgy/raw/master/examples/badgy.bin)
2. Slide the power switch off (up), hold the center button and slide the power switch on (down) at the same time. Your Badgy should now be in OTA mode.
3. Go to http://*BADGY_IP_ADDRESS*:8888/update and upload `badgy.bin`
4. Badgy will now restart with the default firmware
5. You can also do this over USB following the instructions from above

## Using MicroPython with Badgy
Check out our [badgy-python repo](https://github.com/sqfmi/badgy-python)

## Manual Flashing / Recovery (For Rev 1)
* If for some reason your code is crashing and OTA updates isn't working, or if you prefer uploading over serial, you can manually flash the firmware using the programming pads on the PCB. Using a USB-Serial adapter (e.g. FTDI), connect the pins to the pads like so:
```
WARNING: ALWAYS DISCONNECT OTHER POWER SOURCES (E.G. USB/BATTERY) BEFORE CONNECTING SERIAL ADAPTER TO THE PROGRAMMING PADS.
WARNING: AVOID CONDUCTIVE CONTACT (E.G. METALS/LIQUIDS) ON THE EXPOSED PADS TO PREVENT DAMAGE
+------------+-----------+
| USB-Serial | Badgy     |
+------------+-----------+
| RX        =>    TX     |
| TX        =>    RX     |
| GND       =>    0      |
| GND       =>    GND    |
| 3V3       =>    3V3    |
| NC        =>    RST    |
+------------+-----------+
```
The blue LED on Badgy should start blinking during upload. You can use the Arduino IDE or [esptool](https://github.com/espressif/esptool) to flash your firmware.
