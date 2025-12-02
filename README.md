# Blynk-app-joystick-controlled-robo-car


## Project overview

**Title:** Blynk Joystick Controlled Wi‑Fi Robo Car  

This project is a low‑cost Wi‑Fi controlled robot car that can be driven from a smartphone using the **Blynk app joystick**. The NodeMCU/ESP8266 (or ESP32) connects to Wi‑Fi and receives joystick commands from Blynk to control the direction and speed of the DC motors through a motor driver. The system demonstrates basic IoT, mobile control, and DC motor control in a compact, easy‑to‑reproduce platform.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Features

- Full control of the car using a phone (forward, reverse, left, right, stop).  
- Wi‑Fi based control using Blynk cloud (works on same network or via internet, depending on Blynk setup).
- Speed control using PWM (optional slider in the app).  
- Simple, modular wiring that can be extended with LEDs, sensors, or camera later.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Components

**Electronics**

- 1 × NodeMCU ESP8266 (or ESP32) development board  
- 1 × L298N or L293D DC motor driver module  
- 2 or 4 × DC geared motors (for a 2‑WD/4‑WD chassis)  
- 1 × Robot chassis with wheels and caster wheel  
- 1 × Battery pack (7.4–12 V, Li‑ion/Li‑Po or AA pack)  
- 1 × Power switch  
- Jumper wires (male–male / male–female as needed)  
- Small breadboard or screw terminals (for neat wiring)  
- Optional: white LEDs for headlights, resistors

**Software**

- Arduino IDE with ESP8266/ESP32 core installed  
- Blynk library for Arduino/ESP8266/ESP32  
- Blynk mobile app (Android / iOS).

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Hardware connections

**Power**

- Battery + → `Vmot` of motor driver  
- Battery – → GND of motor driver and NodeMCU (common ground)  
- 5 V (from motor driver regulator or separate 5 V module) → NodeMCU `Vin`/`5V`

**Motor driver to motors**

- `OUT1`, `OUT2` → Left motor  
- `OUT3`, `OUT4` → Right motor

**NodeMCU to motor driver**  
(example pin mapping; can be changed in code)

- D5 → IN1  
- D6 → IN2  
- D7 → IN3  
- D8 → IN4  
- D3 (PWM) → ENA (left motor enable)  
- D4 (PWM) → ENB (right motor enable)

**Optional LEDs**

- Any spare GPIO → LED (through ~220 Ω resistor) for headlights or status.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Blynk app configuration

1. Create a new Blynk project (board: **NodeMCU / ESP8266**, connection: **Wi‑Fi**).  
2. Note the **Auth Token** and paste it into the Arduino sketch.  
3. Add widgets:  
   - **Joystick**  
     - Output: Virtual pin `V0`  
     - Mode: 2‑axis, “merge” or “x‑y” mode  
     - Auto‑return to center: ON  
   - (Optional) **Slider**  
     - Output: `V1`  
     - Range: 0–1023 or 0–100 (will be mapped to PWM).  
4. Set your Wi‑Fi SSID and password in the sketch, along with the Blynk auth token.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Working principle

1. On power‑up, the NodeMCU connects to the configured Wi‑Fi network and then to the Blynk cloud.  
2. When you move the joystick in the app, Blynk sends the **X–Y values** of the joystick to virtual pin `V0`.  
3. The code reads these values and classifies them into motion commands:  
   - Joystick up → both motors forward  
   - Joystick down → both motors reverse  
   - Joystick left → left motor slow/stop, right motor forward (turn left)  
   - Joystick right → right motor slow/stop, left motor forward (turn right)  
   - Joystick near center → all motor pins LOW (stop)  
4. If a speed slider is used, its value on `V1` sets the PWM duty cycle on `ENA` and `ENB`, controlling motor speed smoothly.  
5. The result is a responsive Wi‑Fi robot that follows joystick movements in real time.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Suggested repository structure

- `/hardware/`  
  - Fritzing / schematic diagram (`schematic.png`, `wiring.pdf`)  
  - Photos of assembled robot  
- `/code/`  
  - `blynk_wifi_car.ino` (main Arduino sketch)  
- `/docs/`  
  - `README.md` (this description)  
  - `Blynk_setup.md` (screenshots of widget settings)  
- `/media/`  
  - Demo GIF or video link, extra photos

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Possible improvements

- Add obstacle‑avoidance with ultrasonic sensor.  
- Add battery voltage monitoring in Blynk.  
- Add headlights and indicators controlled from the app.  
- Port the code to ESP32 and use advanced features like dual‑core tasks.
