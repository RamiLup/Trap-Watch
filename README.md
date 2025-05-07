  <p style="text-align:center">
    
  # TrapWatch – Humane Wireless Trap Monitoring Module
  <img src="https://github.com/user-attachments/assets/d666cad8-ba5b-4096-8a72-e5c776f0b977" alt="PCB" width="30%" />
  <img src="https://github.com/user-attachments/assets/30e63c30-4d0c-4e8c-a247-698c5da19626" alt="PCB" width="30%" />
  <img src="https://github.com/user-attachments/assets/2249205c-2852-4b6d-9d81-62562e1d01ec" alt="PCB" width="30%" />
  </p>

## 1. Introduction

This IoT system automates routine trap inspections and ensures the timely release of trapped animals by monitoring trap status in real time. A compact PCB with sensors is attached to any standard trap, transforming a low-cost trap into a smart, connected device. Each sensor module communicates with a touchscreen control panel over a range of up to 50 meters, reporting:
* Instant alerts: Visual and audio notifications the moment a trap is triggered
* Keep-alive updates: Regular status reports on battery level, temperature, and connectivity
* Operational reliability: Continuous monitoring even when no traps are triggered.

The sensor’s rechargeable battery delivers over 6 months of uninterrupted operation. This allows traps to be deployed in hard to reach locations without daily visits, and ensures prompt and humane treatment for any trapped animal.
This project demonstrates the development of an IoT concept, born from identifying a problem and applying a creative, technology-driven solution, taking it from idea to a functional, end-to-end system. It integrates hardware design, embedded programming, and user interface development, with careful attention to mechanical integration, power efficiency, and robust debugging.
<br>

---
## 2. Hardware Overview
**Transmitter:**
<br>
<img src="https://github.com/user-attachments/assets/ce93e67f-33f4-49e3-b9a6-63a144c1d69a" alt="PCB" width="33%" />
<img src="https://github.com/user-attachments/assets/a3673dd3-7a78-448d-b526-0889c025a63d" alt="PCB" width="33%" />

<br>

*   Custom designed PCB (designed with EasyEda, manufactured with JLCPBC)
*   ESP32-S3 Xiao Seeed (from Seeed Studio)
*   VL6180X Time-of-Flight Sensor
*   Magnetic reed switch (door sensor) for trap door state detection
*   Magnetic 4pin connector (female type) for connecting to the panel serial port


The PCB was designed to a minimal size, allowing it to fit onto the bait chamber of standard humane mouse traps <br>
It holds two daughterboards: ESP32-S3 Xiao , VL6180X ToF sensor and has dimensions of 44mm x 18mm.

<br>

# Battery Life Calculation

| Mode                     | Current    | Time per Day          | mAh per Day                            |
|--------------------------|------------|-----------------------|----------------------------------------|
| **Deep sleep**           | 0.014 mA   | 24 h                  | 0.014 mA × 24 h = 0.336 mAh            |
| **Active 6× wake-ups day** | 100 mA     | 6 × 0.7 s = 4.2 s      | 100 mA × (4.2 s ÷ 3600 s/h) = 0.117 mAh  |
| **Total per day**        |           |                   | **0.336 + 0.117 = 0.453 mAh/day**      |

Estimated runtime with a 100 mAh battery: 100 mAh / 0.453 mAh/day ≈ 221 days
<br>

---
**Receiver:**
<br>
- CrowPanel 5.0"-HMI ESP32 Display (from Elecrow)
- RTC DS3231 Real-Time Clock (I2C Interface)
- 1000mAh Li-ion battery 
- Custom cable for debug and configuration with magnetic 4 pin connector (male type)

The central hub for viewing all trap statuses, managing the system and setting up new sensors is based on a CrowPanel 5.0" HMI ESP32 Touch Display platform providing a clear and interactive user interface.
I have separated all functionality into three screens:

<br>

---
## **3. Software Design:**
Fast loading, responsive design, and a clear interface for all three screens
<br>

**Main Screen:**
<p style="text-align:center">
<img src="https://github.com/user-attachments/assets/bad19b55-8424-42fc-9575-6b6aef9a9c24" alt="PCB" width="33%" />
</p> 
This screen shows the current state and information for all traps based on the last received ESP-NOW packet. Each received update includes:
<br>
Battery level, RSSI, Internal chip temperature, Number of resets (trap wake-ups since last reset), connected / disconnected state and an image indication if trap was activated lately.
<br>
<br>

**Config Screen:**
<p style="text-align:center">
<img src="https://github.com/user-attachments/assets/b197ed47-5b77-4e72-9553-2767c38daf01" alt="PCB" width="33%" />
</p>
This screen allows users to wake up a trap into configuration mode, set its deep sleep interval and ToF sensor run time, and then return it to operational mode.
<br>
<br>

**Logs Screen:**
<p style="text-align:center">
<img src="https://github.com/user-attachments/assets/6ffefc3f-fdc0-46b9-b60d-b48b7b701a2c" alt="PCB" width="33%" />
</p>

**1. Serial Port:** Shows messages from the traps (as the trap's ESP32 has no display). <br>
**2. Last Events:** Shows all recent events, displaying the trap number first, followed by the event description <br>
**3. Local:** Shows internal system information and debug messages <br>
<br>

---

## **How it works:**
Before first use, each sensor must first be connected to the monitoring panel using the serial cable with the magnetic connector:
<img src="https://github.com/user-attachments/assets/9736b9ac-e0bd-476a-8479-04180f45cfa7" alt="PCB" width="33%" />
<img src="https://github.com/user-attachments/assets/948e4e83-70bb-45d9-bbc9-48a931b5e4b6" alt="PCB" width="33%" />

<br>

The panel is used to update the trap ID [1-4], deep sleep interval [1 min - 1 day], and ToF sensor running time [25 sec - 75 sec].
This configuration takes approximately 5 seconds. Connecting the board to the panel is straightforward using the magnetic connector::
<br>

<img src="https://github.com/user-attachments/assets/c11529fc-daeb-47db-9e62-6d2ffd50799a" alt="PCB" width="33%" />

<br>

The configured PCB is then inserted into the right side of the trap's bait chamber.

<br>

<img src="https://github.com/user-attachments/assets/0e243b3c-5bb2-4a24-8230-94e8bbbdb624" alt="PCB" width="33%" />
<br>

The trap is ready and can be placed where needed.
According to the configured settings, the trap transmits its status at every wake-up, based on the configured deep sleep interval.

<br>

---

## **Getting an alert:**
When a trap detects a catch (triggered by both internal sensors), it sends an ESP-NOW message to the panel. The panel then plays a short audio alert and displays a yellow exclamation mark next to the relevant trap's image. The user can clear this exclamation mark by pressing it.

https://github.com/user-attachments/assets/ccbb180f-6124-4686-a10b-8803e0a97d91

---
