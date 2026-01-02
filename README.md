🧀 Cheese Chamber Controller (ESPHome)

This project is an ESPHome-based cheese aging chamber controller designed for slow, stable maturation using a freezer and an ultrasonic humidifier.

It prioritizes:

Very slow temperature changes (short cooling bursts, long rest periods)

Controlled humidity cycles

Clear visual feedback (LCD + RGB status LED)

Safety against relay short-cycling

✨ Features
🌡 Temperature Control (Relay 1 – Freezer)

Uses probe temperature (DS18B20)

Adjustable:

Temperature setpoint

Hysteresis band

Maximum ON time per cycle (default: 5 min)

Minimum OFF time between cycles (default: 10 min)

Immediate cutoff if temperature drops below the lower band

Designed to avoid aggressive freezer cooling

💧 Humidity Control (Relay 2 – Ultrasonic Humidifier)

Uses ambient humidity (DHT22)

Adjustable:

Humidity setpoint

Hysteresis band

Maximum ON time per cycle (default: 5 min)

Minimum OFF time between cycles (default: 10 min)

Immediate cutoff if humidity exceeds upper band

Ideal for ultrasonic mist makers

🖥 LCD Display (20x4 – I²C)

The LCD shows real-time chamber status:

Air:12.4C H:84%
Prb:12.1C Sp:12.0
R1:1 COOL R2:0 IDLE
W1:120s W2:300s 14:32


Meaning:

Air → Ambient temperature & humidity

Prb → Probe temperature & setpoint

R1 / R2 → Relay state and mode

COOL, HUM, WAIT, NEED, IDLE

W1 / W2 → Remaining wait time before relay can restart

Time synced from Home Assistant

💡 RGB Status LED (Single LED)

The LED gives instant visual feedback with clear priorities:

Condition	Color	Effect
Freezer fault (relay ON, no power)	🟣 Purple	Strobe
Too hot	🔴 Red	Strobe
Too cold	🔵 Blue	Strobe
Cooling active	🟦 Cyan	Pulse
Humidifying active	🟢 Green	Pulse
Too dry (waiting)	🟡 Yellow	Solid
Too humid	🟦 Blue/Green	Solid
Stable / OK	🌈 Rainbow	Slow

LED can be disabled via Enable Status LED switch.

⚡ Power Monitoring (Optional Safety)

CT clamp measures freezer current

Power < 500 W is ignored

Detects fault condition if:

Freezer relay ON

No real power draw

Triggers purple strobe alert

🛡 Safety & Reliability

10-second startup inhibit (no relay switching after boot)

Enforced:

Minimum OFF times

Maximum ON durations

Protects:

Compressor

Relays

Cheese 😄

🔧 Hardware Summary

ESP32 Lolin S2 Mini

Relay 1 → Freezer

Relay 2 → Ultrasonic humidifier

DS18B20 → Cheese core temperature

DHT22 → Ambient temp & humidity

20x4 I²C LCD (PCF8574)

RGB LED (3 PWM pins)

CT Clamp + ADC (optional but recommended)

🧠 Design Philosophy

This controller is not a thermostat.

It behaves like a cheese affineur:

Gentle corrections

Long pauses

Stability over speed

Perfect for:

Raclette

Tomme

Reblochon-style aging

Natural rind cheeses

🚀 Future Improvements (Optional)

Web dashboard via ESPHome web_server

Data logging / graphs

Fan control

Door sensor

Multi-profile cheese presets
