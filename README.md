  <p style="text-align:center">
    
  # TrapWatch – Humane Wireless Trap Monitoring Module
  <img src="https://github.com/user-attachments/assets/d666cad8-ba5b-4096-8a72-e5c776f0b977" alt="PCB" width="33%" />
  <img src="https://github.com/user-attachments/assets/30e63c30-4d0c-4e8c-a247-698c5da19626" alt="PCB" width="33%" />
  <img src="https://github.com/user-attachments/assets/2249205c-2852-4b6d-9d81-62562e1d01ec" alt="PCB" width="33%" />
  </p>

## 1. Introduction
This wireless IoT system automates routine trap inspections and ensures the timely release of trapped animals by monitoring trap status in real time. A compact sensor module attaches to any standard trap, transforming a low-cost trap into a smart, connected device. Each sensor module communicates with a touchscreen control panel over a range of up to 50 meters, reporting:
* Instant alerts: Visual and audio notifications the moment a trap is triggered
* Keep-alive updates: Regular status reports on battery level, temperature, and connectivity
* Operational reliability: Continuous monitoring even when no traps are triggered.
The sensor’s rechargeable battery delivers over 4 months of uninterrupted operation. This allows traps to be deployed in multiple, hard-to-reach locations without daily visits saving time, reducing disturbance, and ensuring prompt and humane treatment for any trapped animal.

This project demonstrates the development of an IoT concept, born from identifying a problem and applying a creative, technology-driven solution, taking it from idea to a functional, end-to-end system. It integrates hardware design, embedded programming, and user interface development, with careful attention to mechanical integration, power efficiency, and robust debugging.

<br>

---
## 2. Hardware Overview
**Transmitter:**
<br>
<img src="https://github.com/user-attachments/assets/ce93e67f-33f4-49e3-b9a6-63a144c1d69a" alt="PCB" width="33%" />
<img src="https://github.com/user-attachments/assets/a3673dd3-7a78-448d-b526-0889c025a63d" alt="PCB" width="33%" />

<br>

*   Custom designed PCB (desinged with EasyEda, manuafactures with JLCPBC)
*   ESP32-S3 Xiao Seeed (from Seeed Studio)
*   VL6180X Time-of-Flight Sensor
*   Magnetic reed switch (door sensor) for trap door state detection
*   Magnetic 4pin connector (female type) for connecting to the panel serial port


PCB was designed to minimum size, it can fit onto the bait chamber of a standard humane mouse traps<br>
It holds 2 Daughterboards: ESP32-S3 Xiao and ToF VL6180X with dimensions: 44mm x 18mm.
<br>
# Battery Life Calculation

| Mode                     | Current    | Time per Day          | mAh per Day                            |
|--------------------------|------------|-----------------------|----------------------------------------|
| **Deep sleep**           | 0.014 mA   | 24 h                  | 0.014 mA × 24 h = 0.336 mAh            |
| **Active 6× wake-ups a day** | 100 mA     | 6 × 0.7 s = 4.2 s      | 100 mA × (4.2 s ÷ 3600 s/h) = 0.117 mAh  |
| **Total per day**        | —          | 24 h                  | **0.336 + 0.117 = 0.453 mAh/day**      |

Estimated runtime with a 100 mAh battery: 100 mAh / 0.453 mAh/day ≈ 221 days
<br>

---
**Receiver:**
<br>
- CrowPanel 5.0"-HMI ESP32 Display (from Elecrow)
- RTC DS3231 Real-Time Clock (I2C Interface)
- 1000mAh Lion battery 
- Custom cable for debug and Configuration with magnetic 4pin connector (male type)

The central hub for viewing all traps status, managing the system and setting up new sesnors is based on an CrowPanel 5.0" HMI ESP32 Touch Dispaly platform providing a clear and interactive user interface.
I have seperated all functionality in to 3 screens:

<br>

---
## **3. Software Design:**
Fast loading , resposive design and clear interface for all 3 screens
<br>

**Main Screen:**
<p style="text-align:center">
<img src="https://github.com/user-attachments/assets/bad19b55-8424-42fc-9575-6b6aef9a9c24" alt="PCB" width="33%" />
</p> 
Showing current state and information for all traps based on last received ESPNOW packet, each received update includes: <br>
Battery level, RSSI, Internal chip temperature, Number of resets (trap wake ups since last reset), connected / disconnected state 
and indication if trap was activated in the past.
<br>
<br>

**Config Screen:**
<p style="text-align:center">
<img src="https://github.com/user-attachments/assets/b197ed47-5b77-4e72-9553-2767c38daf01" alt="PCB" width="33%" />
</p>
Wake up trap into configuration mode, set traps deep sleep interval and TOF run time and return it back to run mode
<br>
<br>

**Logs Screen:**
<p style="text-align:center">
<img src="https://github.com/user-attachments/assets/6ffefc3f-fdc0-46b9-b60d-b48b7b701a2c" alt="PCB" width="33%" />
</p>

**1. Serial port:** Showing traps messagesp (trap esp32 has no display) <br>
**2. Last events:** showing all last events (trap number first, then the event <br>
**3. Local:** showing internal information and debug messages <br>
<br>

---

## **How it works:**
Before first use, each sensor shall be first connected to the monitoring panel using the serial cable with the magnetic connector:
<img src="https://github.com/user-attachments/assets/9736b9ac-e0bd-476a-8479-04180f45cfa7" alt="PCB" width="33%" />
<img src="https://github.com/user-attachments/assets/948e4e83-70bb-45d9-bbc9-48a931b5e4b6" alt="PCB" width="33%" />

<br>

The panel is used for updating the trapId number [1-4], Deep Sleep time [1min - 1day] and TOF running time [25sec - 75sec]
This configuration takes 5 second and connecting the board to the panel is very easy using the fast magnetic connector:
<br>

<img src="https://github.com/user-attachments/assets/c11529fc-daeb-47db-9e62-6d2ffd50799a" alt="PCB" width="33%" />

<br>

The configured PCB shall be inserted to the right side of the trap bait chamber.

<br>

<img src="https://github.com/user-attachments/assets/0e243b3c-5bb2-4a24-8230-94e8bbbdb624" alt="PCB" width="33%" />
<br>

The trap is ready and can be placed where needed.
According to the configured settings, trap shall transmit status every wake up according to the configured deep sleep time.

<br>

---

## **Getting an alert:**
When trap has detects a catch (after both internal sensors detected it) The trap shall send an ESPNOW mesage to the panel which will play a short audio file followed by a yello excelmation mark that will be shown on the right side of the trap image. The excelmation mark can be cleared by the user by pressing it.

https://github.com/user-attachments/assets/ccbb180f-6124-4686-a10b-8803e0a97d91

---
