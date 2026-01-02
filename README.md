# 🧀 Cheese Chamber Controller (ESPHome + ESP32-S2)

This project is a **DIY cheese aging chamber controller** built with **ESPHome** and an **ESP32-S2 Mini**.  
It controls **temperature** and **humidity** independently using two relays, while monitoring **power consumption** of the freezer with a current clamp.

The system is designed to be:
- Stable (anti relay chatter)
- Adjustable from Home Assistant (sliders / helpers)
- Safe for compressors and ultrasonic humidifiers
- Fully autonomous (interval-based logic, no cloud dependency)

---

## ✨ Features

### 🌡 Temperature Control (Relay 1 – Freezer)
- Uses **cheese probe temperature** (Dallas DS18B20)
- Adjustable **temperature setpoint** and **hysteresis band**
- Minimum ON/OFF time to protect the compressor
- Startup delay to prevent relay toggling at boot
- Status shown on LCD

### 💧 Humidity Control (Relay 2 – Ultrasonic Humidifier)
- Uses **ambient humidity** (DHT22)
- Adjustable **humidity setpoint** and **band**
- Relay drives a **simple ultrasonic mist maker**
- Minimum ON/OFF cycle time (anti rapid switching)
- Status shown on LCD

### ⚡ Power Monitoring
- SCT-030 current clamp on freezer line
- ADC-based current measurement
- Adjustable gain & offset (no recompilation needed)
- Power below 500W is ignored (noise filtering)
- Daily and total energy tracking

### 🖥 LCD Display (20x4 I2C)
Displays:
- Air temperature & humidity
- Cheese probe temperature & setpoint
- Relay states (R1 / R2) with modes (COOL / HUM / IDLE)
- Humidity setpoint and current time
- Backlight controlled by PIR + timeout

### 🏃 Motion Detection
- PIR sensor turns LCD backlight ON
- Backlight turns OFF automatically after timeout

---

## 🧠 Control Logic Overview

### Temperature (Relay 1 – Freezer)

ON  → Probe temp > (setpoint + band)
OFF → Probe temp < (setpoint - band)


Minimum relay cycle time enforced

Boot inhibit delay (10s)

Humidity (Relay 2 – Ultrasonic Mist Maker)
ON  → Humidity < (humidity_setpoint - band)
OFF → Humidity > (humidity_setpoint + band)


Designed for simple ON/OFF ultrasonic humidifiers

Minimum relay cycle time enforced

Boot inhibit delay (10s)

🧰 Hardware Used
Component	Description
ESP32-S2 Mini (LOLIN)	Main controller
DS18B20	Cheese probe temperature
DHT22	Air temperature & humidity
SCT-030	Current clamp for freezer
Relay Module (2-channel)	Relay 1: freezer / Relay 2: humidifier
Ultrasonic Mist Maker	Simple ON/OFF humidifier
LCD 20x4 + PCF8574	I2C display
PIR Sensor	Motion detection
5V Power Supply	ESP32 + relay power
🔌 Wiring Overview
ESP32-S2 Pin Assignment
Function	GPIO
I2C SDA	GPIO2
I2C SCL	GPIO1
LCD (PCF8574)	I2C
DS18B20	GPIO8
DHT22	GPIO6
PIR	GPIO4
Relay 1 (Freezer)	GPIO34
Relay 2 (Humidifier)	GPIO21
CT Clamp (ADC)	GPIO10

⚠️ Safety Note

The SCT-030 clamp is non-invasive, safe for measurement
Relays must be properly rated for mains voltage

Ultrasonic mist maker is switched on the AC side via relay, not powered directly by ESP32

🖥 Home Assistant Integration
All key parameters are exposed as sliders:
Temperature setpoint
Temperature band
Humidity setpoint
Humidity band
Minimum relay cycle time
CT calibration (gain / offset)
No recompilation is needed to tune the system.

🚀 Installation Steps
Flash ESP32-S2 with ESPHome
Connect sensors, relays, and LCD
Add device to Home Assistant
Adjust setpoints and bands
Let the chamber stabilize for 24–48 hours
Fine-tune humidity & temperature cycles

🧀 Designed For
Cheese aging (raclette, tomme, reblochon, etc.)
Controlled curing environments
DIY food fermentation chambers

📌 Notes & Future Improvements
Add fan control (air circulation)
Add humidity exhaust / dehumidifier
Add alarms (relay ON but no power draw)
Optional ESPHome climate entities for UI abstraction

⚠️ Disclaimer

This project controls mains voltage equipment.
You are responsible for ensuring:
Proper electrical isolation
Safe wiring practices
Compliance with local electrical codes

❤️ Credits
Built with:
ESPHome
Home Assistant

Open-source hardware & software

Enjoy your cheese! 🧀
