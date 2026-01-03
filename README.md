🧀 Cheese Controller — ESP32-S2
Temperature & Humidity Controller for Cheese Aging
This project implements an ESPHome-based controller running on an ESP32-S2, designed for cheese aging (raclette, reblochon, tomme, etc.) inside a modified freezer or refrigerator.
The main goal is slow, controlled thermal stability, respecting the high thermal inertia of cheese — without aggressive PID control.

🎯 System Goals
Maintain a stable core (probe) temperature
Avoid overshoot (too cold)
Protect the cheese rind from thermal shock
Regulate humidity
Provide full observability (LCD + Home Assistant)

🧠 Control Philosophy

❌ No PID
✅ Zone-based logic with adaptive timing

Why?

Very high thermal inertia (cheese + air + enclosure)
Long response delays
PID tends to oscillate and overcorrect in this context

🌡️ Temperature Control — Overview
Sensors
Probe (DS18B20 / Dallas)
→ primary control sensor (cheese mass)
Air (DHT22)
→ safety + thermal buffer (not the main control)

Temperature Zones Around Setpoint (SP)
Zone	ΔT = Probe − SP	Behavior
FAR	> Temp FAR delta	Longer cooling cycles
MID	between MID delta and FAR delta	Interpolated behavior
NEAR	< MID delta	Short cycles + long pauses
⏱️ Intelligent Timing (Core of the System)
1️⃣ Dynamic Minimum OFF Time
The closer the probe is to the setpoint, the longer the controller waits before restarting cooling.
R1 NEAR Min OFF
R1 OFF Bonus near (progressive bonus close to SP)
👉 Prevents compressor short-cycling and temperature pumping.

2️⃣ Temperature Slope (dT/dt) Brake
The controller computes the temperature slope in °C per 10 minutes:
d > 0 → probe still cooling → wait longer
d ≈ 0 → stable
d < 0 → probe warming → react sooner
This prevents the classic failure mode:
“Waited too long → temperature starts rising again”

❄️ Air Temperature Safety (Anti-Shock)
Air temperature does not control the system, but protects it.
Latch ON threshold:
air < SP - air_safety_delta
Latch OFF threshold (hysteresis):
air > (SP - air_safety_delta + air_safety_hyst)

Behavior:
Latch ON → compressor blocked
Latch OFF → normal control
👉 Prevents excessively cold air, condensation, and rind damage.

💧 Humidity Control
Simple ON / OFF logic
max_on + min_off
Intentionally gentle and conservative

📟 LCD Display (20×4)

Displays in real time:
Air temperature & humidity
Probe temperature & setpoint
Relay states (R1 / R2)
Remaining wait time
Temperature slope (d = °C/10m)
Clock

🧪 Debug & Observability (Home Assistant)
🔍 Cheese Debug Line (Copy / Paste Friendly)

A text_sensor exposes all key values in a single line:

Air=10.6C 100% | Prb=12.27C | SP=12.00 band=0.50 |
d=0.00C/10m | zone=NEAR |
R1=0 latch=0 (cut=4.5/5.1) |
hot=0 cold=0 | R2=0


➡️ Easy to copy from Developer Tools → States
➡️ Ideal for remote troubleshooting and tuning

🚨 Built-in Safeties
Compressor anti-short-cycle
Immediate shutdown if:
air too cold
probe too cold
Boot inhibition (10 seconds)
Fault detection:
relay ON but no current detected
Safe behavior if sensors return NaN

⚙️ Recommended Settings (12 °C Aging)
Parameter	Value
Temp Band	0.4 – 0.6 °C
Temp MID delta	0.8 – 1.0 °C
Temp FAR delta	2.5 – 3.0 °C
R1 FAR Max ON	~360 s
R1 NEAR Max ON	~90 s
R1 NEAR Min OFF	600 s
R1 OFF Bonus near	300–600 s
Probe Fast Drop	1.0 °C / 10 min
Air safety delta	7–8 °C
Air safety hysteresis	0.4–0.8 °C
🧀 Expected Behavior

Slow, controlled temperature convergence
No overshoot
Minimal compressor cycling
Healthy, stable rind development
Predictable, explainable control behavior

📌 Final Notes
This controller is designed for:
Artisan cheese aging
Experimental but controlled environments

Understanding and tuning over blind automation

It prioritizes readability, predictability, and robustness over complexity
