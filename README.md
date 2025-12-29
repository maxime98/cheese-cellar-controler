🧀 Cheese-Controleur

ESP32-based Temperature & Energy Controller for Cheese Aging (Cheese Cave / Modified Freezer)

📌 Overview

Cheese-Controleur is a DIY controller designed to convert a standard freezer into a cheese aging chamber (cheese cave).
It monitors temperature, humidity, energy consumption, controls the freezer via relay, and provides visual feedback using an OLED display and an RGB status LED.

The project is built using:

ESP32-S2 Mini (LOLIN S2 Mini)

ESPHome

Home Assistant integration

It is designed to be:

Reliable

Safe for compressors

Fully observable (temperature + power + faults)

✨ Features
🌡 Environmental Monitoring

Air temperature & humidity (DHT22 / AM2302)

Product temperature via probe (DS18B20 on bottle of water)

OLED display with live values

❄ Freezer Control

Relay-controlled freezer power

Designed for external thermostat logic (ESPHome / Home Assistant)

Fault detection (relay ON but no current)

⚡ Energy Monitoring

Current measurement using SCT-013-030 current clamp

Estimated power (W)

Daily energy consumption (kWh/day)

Total cumulative energy (kWh)

🚨 Safety & Alerts

Detects when the freezer is commanded ON but draws no current

Visual fault indication (purple strobe LED)

Ready for Home Assistant notifications

💡 Visual Feedback

RGB LED status meanings:

🔴 Red strobe → Too hot

🔵 Blue strobe → Too cold

💠 Cyan pulse → Cooling (relay ON)

🌈 Rainbow → Stable

🟣 Purple strobe → Fault (relay ON, no current)

🖥 User Interaction

PIR motion sensor turns OLED screen ON

Configurable screen timeout

Live clock display

🧱 Hardware Used
Core Components

ESP32-S2 Mini (LOLIN S2 Mini)

SSD1306 OLED (128×64, I²C)

DHT22 / AM2302 (air temperature & humidity)

DS18B20 (probe temperature)

SCT-013-030 (30A / 1V) current clamp

2-Channel Relay Module (5V, optocoupled, LOW-level trigger)

RGB LED (common anode)

PIR Motion Sensor (HC-SR501 or similar)

🔌 Electrical Wiring Summary
Power

ESP32 powered via VBUS (5V)

Shared 5V supply for relays and ESP32

Common ground across all components

Sensors
Sensor	Connection
DHT22	GPIO6
DS18B20	GPIO8 (OneWire)
PIR	GPIO4
SCT-013-030	GPIO10 (ADC)
OLED (I²C)
Signal	GPIO
SDA	GPIO1
SCL	GPIO2
Relays
Relay	GPIO
Freezer Relay	GPIO34
Auxiliary Relay	GPIO21

⚠ Relays are inverted (LOW = ON) for compatibility with optocoupled modules.

RGB LED (PWM)
Color	GPIO
Red	GPIO40
Green	GPIO38
Blue	GPIO36
🔧 SCT-013-030 Current Clamp Wiring

The SCT-013-030 outputs an AC voltage (~1V max) and must be biased for ESP32 ADC input.

Required Bias Circuit

Voltage divider: 2 × 100kΩ (3.3V → midpoint → GND)

Offset voltage: ~1.65V

Capacitors:

10µF (stability)

100nF (noise filtering)

Notes

Clamp must be placed on ONE conductor only (HOT wire)

Never connect/disconnect the clamp under load

Short wires recommended to reduce noise

🧠 Software Architecture
Firmware

ESPHome

Framework: ESP-IDF

OTA updates enabled

Home Assistant API enabled

Main Logic

Read sensors (temperature, humidity, current)

Compute estimated power and energy

Detect faults (relay ON + no current)

Update OLED display

Drive RGB LED based on system state

Expose all entities to Home Assistant

📈 Home Assistant Integration

All sensors, switches, and energy entities are automatically exposed to Home Assistant:

Power & energy usable in Energy Dashboard

Fault sensors usable for notifications

Setpoints adjustable from UI

🚀 Setup Steps

Assemble hardware and wiring

Install ESPHome

Flash ESP32 with provided YAML

Add device to Home Assistant

Calibrate SCT-013-030 using a known resistive load

Configure automations / thermostat logic (optional)

⚠ Calibration (Important)

The SCT-013 calibration values are placeholders.

To calibrate:

Connect a known load (e.g. 1500W heater)

Measure real current (A = W / 120V)

Adjust calibrate_linear values in ESPHome

🛣 Future Improvements

True thermostat with anti-short-cycle protection

Compressor minimum OFF time

Voltage measurement for real power (PF-aware)

Web UI status page

Data logging to InfluxDB / Grafana

📜 License

This project is open-source and provided as-is for educational and DIY use.
Use at your own risk when working with mains voltage.
