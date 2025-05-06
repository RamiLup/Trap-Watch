<h1 style="text-align:center">TrapWatch – Humane Wireless Trap Monitoring Module</h1>


<p style="text-align:center">
  <img src="https://github.com/user-attachments/assets/2249205c-2852-4b6d-9d81-62562e1d01ec" alt="PCB" width="30%" />
  <img src="https://github.com/user-attachments/assets/d666cad8-ba5b-4096-8a72-e5c776f0b977" alt="PCB" width="30%" />
  <img src="https://github.com/user-attachments/assets/30e63c30-4d0c-4e8c-a247-698c5da19626" alt="PCB" width="30%" />  
</p>


## 1. Introduction
Checking animal traps in hard-to-reach places like under the house is a chore. It's inconvenient, often unpleasant, and frequent manual checks can leave scent trails that actually deter the very animals you're trying to catch.
More importantly, traditional traps require you to check them manually and often, which means a trapped animal might remain confined for hours or even days before you discover it. Leaving an animal trapped for so long is inhumane, regardless of the trap type.

To solve this, I developed a wireless IoT system that monitors trap status. It transforms the simplest, low-cost traps into smart, connected devices.
Attach the sensor to any standard trap, and monitor its status via a compact touchscreen control panel. You can deploy it across various trap types, just keep in mind that the system (trap to monitor) range is about 50 meters.
When a trap is triggered, the system sends an instant message to the panel, which triggers visual and audio alerts, allowing for the timely and humane release of captured animals. Furthermore, to ensure continuous operation, each sensor transmits regular updates (keep-alives) on its status, battery level, and temperature, so you know the system is operational even when there’s no activity.
The compact size of the sensor includes a rechargeable battery providing more than 4 months of operation.

## 2. Hardware Overview

## Transmitter (Trap Module) Designed mechanically to fit snugly onto the back bait chamber of standard humane mouse traps.

*  Main pcb: 44mm x 18mm and it holds 2 Daughterboards: ESP32-S3 Xiao and ToF VL6180X.

>>> image of pcb and all other parts including MAGNETS with name and signs. <<<



## 4. Receiver Side (Trap Monitoring Panel)

The central hub for viewing trap status, managing the system and setting up new sesnors
Based on an **Elecrow ESP32-S3 Touch Screen (7-inch, 800x480 resolution)** platform providing a clear and interactive user interface.
Panel has 3 screens:
Main
Setting
Logs

((( SHOW SMALL IMAGES IN ONE LINE OF THEM ALL # SCREENS)






* **How it works:**
First, each new sensor shall be connected to the Trap monitoring panel before first use.
the configuration takes 5 second thanks to the fast magnetic connector.
[[show gif of connecting the esp to the panel with connector]]

after the sensor assained with the TrapId its inserted to the right side of traps bait chamber and the trap can be placed where needed.

The trap will transmit configiration and status every wake up (the deep sleep time can vary between 1min and up to once a day to save energy if needed).

SHOW IMAGE WITH EXAMPLE ON SCREEN AND ON SCREEN SETTINGS >> see wake up update for trap 2 and 3 in this image:

show the screen image with arrows and text: "Real time clock", trap number, deep sleep time, period , las seen

show a video when mouse is traped with 2 screen one for the trap catchin the mouse one for the monitor with AUDIO.

when trap has detected a catch (both sensors approved it) it will send a notofication to the panel which will play a short audio file.

a yello excelmation mark will be shown on the right side of the trap image.
this excelmation mark can be cleared by pressing it.

* **Basic Features List**
Custom PCB designed to snap onto the back of any standard mouse trap
On-board configuration & debug fast connect using magnetic connector  
VL6180X Time-of-Flight module for in trap distance messauress
powered on only when trap door is closed, with adjustable run-time (stored in Non vailoted RAM)
Magnetic reed switch to detect trap door state 
Battery level monitoring 
Internal ESP32-S3 temperature reading  
Configurable deep-sleep interval (value stored in Non vailoted RAM)
Configurable ToF run time for incresing reability when detecting small animals (value stored in Non vailoted RAM)
configurable TRAPID number (value stored in Non vailoted RAM)
Persistent settings using NVS

communication:
It is bidirectional ESP-NOW immplimentation protocol but only the trap initiliate communication session.
Panel can send command an espnow packet only as a respond to received espnow packet from the trap.
Each wakeup or door state change the trap tranmite the espnow packet that includes: Uinque TRAP ID, current door state, ToF run time, Deep Sleep time, battery level, chip temperature, channel rssi  




* **More features:**
* 
# fast magentic connector for serial port setting from the trap monitor, its make it very easy to connect and disconnect the trap from the configuration cable.

# I2C RTC is connected to the panel providing accurate time logs and information.

# Panel backup battery (can work for an hour without charger, any no dsta will be lost, since the trap keeps it all in the NVS memory and transmitting it on each wake up to the receiver).

# I2S Speaker using the onboard audio chip, each time a trap deteced a sucsesssfue catch, a trupmt sound will be sound once from the panel speaker.
(each trap can sound the trumot only one time per monitor restart).



## Trap monitor screen has 3 screens:

show one image with 3 small screens add names in the image for every each one of them:

ADD ALSO image of screen and trap to main inamge
Add video operating the trap

## Main: [IMAGE]

showing the current all traps state which include for each state the last received values of:
battery , rssi , internal chip temperature, number of resets (wake ups that the trap did since last reset), connected / disconnected icon
(if trap did npt send message when it should have, monitor calculate the if: "current time" - "last time" > "deep sleep time" 

## Config: [IMAGE]
wake up trap to config mode / set trap to deep sleep mode
configure trap using serial port (id, sleep time, tof time)

## Logs: [IMAGE]
1. Serial port: show all data received from the serial debuf port of the trap (trap has no display). 
2. Last events: show all last events. showing the trap number first, then the event name.
3. Local: showing pared data (from espnow/serial port)




## 5. Features Highlight

**Main Hardware Parts:**
TRAP SIDE:
*   Custom-designed Sensor PCB (desinged with EasyEda, manuafactures with JLCPBC)
*   ESP32-S3 Xiao Seeed (from Seeed Studio)
*   VL6180X Time-of-Flight Sensor
*   Magnetic Reed Switch (Door Sensor)
*   Magnetic Connector (Power/Debug/Config)

MONITOR
- **CrowPanel 5.0"-HMI ESP32 Display** (from Elecrow)
- **VL6180X** Time-of-Flight proximity sensor (From DFRobot)
- **Magnetic reed switch** (normally-open) for trap door detection  
- **RTC** DS3231 Real-Time Clock (I2C Interface)
- **Custom Debug and Configuration Cable 

SHOW IMAGES OF ALL PARTS


- 




**Software & System:** <> Some words about the firmwares:

*   **Wireless Communication:** ESP-NOW protocol for energy efficiens ,bidirectioanl communication 
*   **Ultra-Low Power:** Deep Sleep modes (Timer, EXT0, EXT1) with RTC memory state retention.
*   **Intelligent Sensing:** ToF sensor activated *only* post-closure for power saving.
*   **Persistent Configuration:** Settings stored in NVS (Preferences library).
*   **Remote Management:** Configure Trap ID, Sleep Time, ToF Time via the Panel UI.
*   **Real-time Status:** Immediate updates on trap door changes and motion detection.
*   **Telemetry Data:** Battery Voltage, Chip Temperature, Motion Count, Uptime Counter.
*   **Robust Communication:** Handles ESP-NOW send/receive callbacks and acknowledgments. Uses JSON for structured serial communication.
*   **Dual-Mode Operation:** Normal low-power monitoring mode and a dedicated Serial Configuration/Debug mode.
*   **Mechanical Integration:** Sensor PCB designed specifically to fit standard traps.
*   **Debugging Support:** Direct serial log streaming to the panel via magnetic connector.
*   **Firmware:** Developed using the Arduino framework for ESP32.
*   **Arduino IDE** project structure (two `.ino` sketches)  
*   **LVGL 8** UI library for responsive graphics & touch  
*   **ESP-NOW** protocol for ultra-low-latency, infrastructure-free RF comms  
*   **ArduinoJson** for compact JSON messaging & configuration  
*   **State machines** for TOF power-cycling & trap state transitions  
*   **Deep sleep** & wake-up routines to extend battery life  
*   **LittleFS** for on-device logging and future over-the-air updates  
*   **Serial debug** output on both sides for easy troubleshooting




magnetic connector
Just attach a magnetic sensor to any standard trap, and monitor its status via a compact touchscreen control panel. You can deploy multiple sensors across various trap types, up to 30 meters away from the monitor.

When a trap is triggered, the system sends instant visual and audio alerts, enabling timely response or humane release. Each sensor also transmits regular updates on trap state, battery level, and temperature—so you know everything is working, even when there’s no activity.


Checking animal traps (mice in my case) in inconvenient locations like under the house is a chore. 
It's difficult, unpleasant, and regular trap checks often leave scent trails or other traces behind, which can deter the animals you aim to catch.
I have finished my new wireless #IoT system that can detects mice activity in the traps remotely.
it can monitor traps efficiently, reliably by transforming standard traps even the basic, inexpensive models readily available online for just a few dollars into smart, connected devices. Attach magnet sensor to any trap, and monitor its status remotely via a simple handheld touchscreen interface. Deploy it anywhere (up to 30m form the monitor) and use as many sensors as you need, across any type of trap.
Get instant visual and audio alerts when a trap triggers. This immediate trap notification enabling the timely release of captured animals. For peace of mind, each sensor regularly reports its status, battery level, and temperature back to the monitor, confirming the system is operational even when there's no activity. 



When you need to place traps in hard to reach places under the house, in crawl spaces or behind heavy furniture physically checking them regulary can be unpleasant
but more important then that, by checking the traps regulary you might leave traces which might deter the animals you’re trying to catch. 

This project helps by turning any standard trap into a smart, efficent battery powered sensor with easy to use mobile touchscreen monitor. 

it was designed for scale and flexibility, you can deploy as many traps as needed and of any kind your situation requires. 
Whenever a trap is triggered, you’ll receive an immediate visual and audio alarm on the touchscreen panel. 

To make sure the system is running and working, even when nothing happens, each trap “pings” the panel every few hours with a keep-alive message, 
that include all the internal information from the sesnor (temperature, battery level, and more).




## 1. Introduction

Ever set up mouse traps in hard-to-reach places like attics, crawl spaces, or behind bulky furniture? Checking them regularly becomes a chore, and you often don't know if you've caught anything until... well, until it's obvious. This project tackles that exact problem!
A compact, end-to-end solution to keep tabs on your traps—without the daily trek to check them. This project combines a tiny trap-mounted PCB sensor with a full-featured Elecrow ESP32-S3 touchscreen panel to report trap state, distance readings, battery level and more via ESP-NOW.  
When you deploy mouse traps in hard-to-reach spots, you shouldn’t have to crawl under furniture or climb into attics just to see if you’ve caught anything. That’s why I designed a custom Arduino-based PCB that snaps onto the back of any standard mouse trap and reports **trap state**, **battery voltage**, **internal temperature**, **RF signal strength** and **time-of-flight distance** back to a touchscreen panel—so you get instant alerts the moment a mouse is caught. Every hardware and software decision was made with mechanical fit, power consumption and reliable communication in mind.

Since the eso is in deep sleep mode, until the trap door state has changedm the small 100mAh battrey which is located under the TOf pcb, can power my trap for about 5 months for single charge.
(calculation is based on current testing i have done and the follwoing calculation: every 4 hours the esp wakes up for 0.75s and cosume ~80ma. when the esp is in deep sleep mode it takes
~16uA). 



## 2. TL;DR

*   **What:** A wireless system using ESP-NOW to monitor mouse traps.
*   **How:** A custom ESP32-S3 based sensor PCB (ToF, magnetic switch) attaches to traps, sending data to an ESP32-S3 touch-screen panel.
*   **Why:** Get real-time trap status (Open/Closed/Motion Detected!), battery levels, and configure traps remotely without needing physical access. Features deep sleep for long battery life.

- **Trap sensor**: ultra-small PCB with VL6180X Time-of-Flight + magnetic switch + battery monitor + ESP32-S3, fits on back of a standard mouse trap  
- **Monitoring panel**: Elecrow 5″ 480×800 ESP32-S3 touchscreen running LVGL UI, shows live trap status, logs & configuration  
- **Wireless comms**: ESP-NOW + JSON for sub-second updates, zero-infrastructure RF  
- **Power-smart**: configurable deep-sleep + on-demand TOF sensing to maximize battery life  
- **Extensible & debug-friendly**: on-screen settings, LittleFS logs, serial debug output  





This project is a testament to building practical, end-to-end IoT solutions, combining hardware design, embedded programming, wireless protocols, and user interface considerations to solve a common household annoyance effectively.








*   **Trap Door Sensing:** Utilizes a **magnetic reed switch** to reliably detect if the trap door is open or closed.
*   **Motion Detection:** Incorporates a **VL6180X Time-of-Flight (ToF) sensor**. Crucially, the ToF sensor is **only powered on *after* the trap door closes**, significantly saving power. It then monitors for movement within the trap for a configurable duration.
*   **Low-Power Wireless:** Uses **ESP-NOW** protocol for efficient, connectionless communication directly with the monitoring panel.
*   **Power Optimization:** Employs **deep sleep** modes, waking up periodically (configurable timer) or instantly upon events (trap door state change via EXT0 interrupt, configuration mode pin via EXT1). RTC memory (`RTC_DATA_ATTR`) preserves essential state across sleep cycles.
*   **Telemetry:** Sends status updates including Trap ID, door state (Open/Closed), motion detection count, internal chip temperature, battery voltage, and current configuration settings.
*   **Configuration & Debugging:**
    *   Features a **magnetic connector** for easy, temporary connection to the monitoring panel or a computer.
    *   Supports a **configuration mode** (activated by a dedicated GPIO pin) allowing parameter changes (Trap ID, Sleep Duration, ToF Active Time) via a **Hardware Serial** interface over the magnetic connector.
    *   Settings are persistently stored in **Non-Volatile Storage (NVS)** using the Preferences library.
    *   Sends detailed logs and JSON status updates over serial in configuration mode.






# Energy - Battery Life Calculation

| Mode                     | Current    | Time per Day          | mAh per Day                            |
|--------------------------|------------|-----------------------|----------------------------------------|
| **Deep sleep**           | 0.014 mA   | 24 h                  | 0.014 mA × 24 h = 0.336 mAh            |
| **Active (6× wake-ups)** | 100 mA     | 6 × 0.7 s = 4.2 s      | 100 mA × (4.2 s ÷ 3600 s/h) = 0.117 mAh  |
| **Total per day**        | —          | 24 h                  | **0.336 + 0.117 = 0.453 mAh/day**      |

## Estimated Runtime With a 100 mAh battery: 100 mAh / 0.453 mAh/day ≈ 221 days
---



I designed and built a **two-part wireless monitoring system** to bring intelligence to standard humane mouse traps. It consists of:

1.  A **compact to fit the standard human mouse trap, small designed pcb sensor module** that retrofits onto a standard trap.
2.  A central **touch-screen monitoring panel** that displays the status of multiple traps.

The goal was to create a reliable, low-power solution that provides real-time updates, eliminating the need for constant manual inspection. This system not only tells you if a trap door has closed but also uses a Time-of-Flight (ToF) sensor to detect if there's actual movement *inside* the closed trap. It monitors battery levels and allows for remote configuration, showcasing a blend of hardware design, embedded software development, wireless communication, and practical problem-solving.

This project demonstrates the ability to take a concept from idea to a functional prototype, considering mechanical integration, power efficiency, and robust debugging capabilities.



PANEL:


*   **Wireless Reception:** Listens for **ESP-NOW** broadcasts from multiple trap sensor modules.
*   **Status Display:** Presents a clean overview of all registered traps, showing:
    *   Trap ID
    *   Current Status (e.g., "Open", "Closed", "Motion Detected!")
    *   Battery Level (%)
    *   Last Update Time
    *   (Potentially) RF signal strength indicator (RSSI from ESP-NOW)
*   **Configuration Interface:** Allows users to remotely configure parameters for each trap via the touch screen:
    *   Set unique Trap IDs.
    *   Adjust the deep sleep interval.
    *   Define the duration the ToF sensor remains active after the trap closes.
    *   Clear the "Motion Detected" flag.
*   **Debugging Hub:** When a trap sensor is connected via its magnetic connector, the panel can display **real-time serial logs** streamed directly from the sensor module, greatly simplifying debugging and setup.
*   **Communication Protocol:** Parses incoming ESP-NOW data structures and JSON messages (for configuration acknowledgments/updates). Sends configuration commands and acknowledgments back to the sensors via ESP-NOW.


## 4B xxxxx Receiver Side (Monitoring Panel)

- **Elecrow ESP32-S3 HMI panel** (5″ TFT-LCD, 480×800) with PSRAM for smooth graphics  
- **LVGL-powered UI**  
  1. **Main screen**: real-time trap status grid (up to 4 sensors), including distance icons, bell symbols, trap resets count, RF signal bars & battery level  
  2. **Config screen**: adjust trap IDs, deep-sleep time & TOF runtime on-the-fly  
- **Data logging**  
  - **Local**, **remote** and **event** logs (20 msgs each) in rotating queues  
  - **LittleFS** storage for persistent logs and over-the-air updates  
- **Communication**  
  - ESP-NOW receive callback registers each incoming sensor packet  
  - JSON parsing & UI updates in under 50 ms  
- **Developer tools**  
  - On-screen serial log window  
  - PSRAM diagnostics & free memory print-outs at startup 




