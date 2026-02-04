# ESP32 SWD Flasher for nRF52
This software makes it possible to Read and Write the internal Flash of the Nordic nRF52 series with an ESP32 using the SWD interface.
A tool to exploit the APPROTECT vulnerability is included as well.

**Supported boards:** ESP32, ESP32-S3 (4MB, 8MB, or 16MB flash), ESP32-C3. See `platformio.ini` for environment selection.

### Original project (atc1441)

Forked from [atc1441/ESP32_nRF52_SWD](https://github.com/atc1441/ESP32_nRF52_SWD) by Aaron Christophel (ATCnetz.de).

- **Support the original author:** [PayPal](https://paypal.me/hoverboard1)
- **Demo videos:**

[![YoutubeVideo](https://img.youtube.com/vi/tMPD0kBG_So/0.jpg)](https://www.youtube.com/watch?v=tMPD0kBG_So)
[![YoutubeVideo](https://img.youtube.com/vi/Iu6RoXRZxOk/0.jpg)](https://www.youtube.com/watch?v=Iu6RoXRZxOk)

### Pin Connections

**ESP32 (default):** SWDCLK=GPIO21, SWDIO=GPIO19, NRF_POWER=GPIO22, GLITCHER=GPIO5, OSCI=GPIO34

**ESP32-S3:** SWDCLK=GPIO41, SWDIO=GPIO42, NRF_POWER=GPIO6, GLITCHER=GPIO4, OSCI=GPIO5

To flash an nRF52 connect the following:
- nRF52 **SWDCLK** to ESP32 **swd_clock_pin** (see platformio.ini for your board)
- nRF52 **SWDIO** to ESP32 **swd_data_pin**
- nRF52 **GND** to ESP32 **GND** to N-Channel MOSFET **GND** (Optional: O-scope **GND Clips**)
- Then power the nRF52 as needed

To bypass the Readout protection (APPROTECT) of an nRF52 connect all of the above and the following:
- nRF52 3.3V Power **VDD** to ESP32 **NRF_POWER** pin (Optional: O-scope **Channel 2 Probe**)
- N-Channel MOSFET **PWM+** to ESP32 **GLITCHER** pin (as shown)
- N-Channel MOSFET **VOUT-** to nRF52 **DEC1** (as shown) (Optional: O-scope **Channel 1 Probe**)
- Then power the nRF52 as needed

### Required Hardware

- ESP32, ESP32-S3, or ESP32-C3 Development Board
- N-Channel MOSFET Board (for glitcher)
- nRF52 Series Board (nRF52832, nRF52840, etc.)
- Optional: Oscilloscope

### HowTo

1. Use **Visual Studio Code with PlatformIO** to compile and upload the project. (Arduino IDE is not supported.)
2. Set WiFi credentials in `src/web.cpp` (`ssid` and `password`).
3. Adjust pin assignments in `platformio.ini` for your board if needed.
4. Build and upload:
   - **ESP32 (4MB):** `pio run -e ESP32_nRF52_SWD`
   - **ESP32-S3 (8MB):** `pio run -e ESP32-S3_8MB_nRF52_SWD`
   - **ESP32-S3 (16MB):** `pio run -e ESP32-S3_16MB_nRF52_SWD`
   - **ESP32-C3 (4MB):** `pio run -e ESP32-C3_nRF52_SWD`
5. Upload `data/index.htm` via the web editor at `http://<ip-address>/edit` (login: admin/admin).

**Storage:** Uses LittleFS with large-storage partition schemes (~2.9MB on 4MB ESP32, ~14MB on 16MB ESP32-S3) for firmware backup and flashing. Sufficient for full nRF52840 (1MB) backup + new firmware. 



### ESP32 Glitcher schematic:

<img width="800" alt="" src="https://github.com/atc1441/ESP32_nRF52_SWD/blob/main/ESP32_nRF_glitcher_schematic.jpg">

#### nRF52832 Glitch Tip, way better results with these 2 caps removed
<img width="800" alt="" src="https://github.com/atc1441/ESP32_nRF52_SWD/blob/main/nRF52832_glitchtip.jpg">


Credits go to LimitedResults for finding the Power glitching Exploit: 
Part 1: https://limitedresults.com/results/nrf52-debug-resurrection-approtect-bypass
Part 2: https://limitedresults.com/results/nrf52-debug-resurrection-approtect-bypass-part-2
Pocket Glitcher: https://limitedresults.com/results/the-pocketglitcher
