Smart Parking Barrier System
An automated parking barrier that detects an approaching vehicle, raises the barrier arm, waits for the vehicle to pass through, then lowers the barrier automatically — no manual operation needed.

Features
Detects approaching vehicles using an ultrasonic entry sensor
Automatically opens the barrier (servo-driven arm)
Detects when the vehicle has fully passed using a second exit sensor
Automatically closes the barrier once the lane is clear
Serial Monitor debug output for live distance and system state
Components Required
Component	Quantity
Arduino Uno/Nano	1
HC-SR04 Ultrasonic Sensor	2
SG90 Servo Motor	1
Breadboard	1
Jumper wires	~15
Circuit Connections
Entry Sensor (HC-SR04)

Pin	Arduino
VCC	5V
GND	GND
Trig	Pin 9
Echo	Pin 8
Exit Sensor (HC-SR04)

Pin	Arduino
VCC	5V
GND	GND
Trig	Pin 7
Echo	Pin 6
Servo Motor

Pin	Arduino
Signal	Pin 10
VCC	5V (or external 5V supply)
GND	GND (must share common ground with Arduino)
All VCC pins connect to the same 5V rail, and all GND pins connect to the same ground rail. If the servo jitters or causes resets, power it from a separate 5V source while still sharing a common ground with the Arduino.

How It Works
Idle — Barrier stays closed, both sensors continuously check for objects.
Vehicle detected at entry — Entry sensor reads a distance below the threshold (default 15 cm) → barrier opens.
Open & hold — Barrier stays open for a short safety delay, giving the vehicle time to move under it.
Waiting for clearance — System watches the exit sensor: it must first detect the vehicle, then detect it has moved away.
Closing — Once the vehicle has fully passed the exit sensor, the barrier lowers and the system returns to idle.
Configuration
Adjust these values at the top of the sketch to tune behavior:

const int DETECT_THRESHOLD_CM = 15;       // detection distance in cm
const int BARRIER_OPEN_ANGLE  = 90;       // servo angle when open
const int BARRIER_CLOSED_ANGLE = 0;       // servo angle when closed
const unsigned long BARRIER_OPEN_HOLD_MS = 3000; // hold time after opening (ms)
Setup Instructions
Wire the components as described above.
Open smart_parking_barrier.ino in the Arduino IDE.
Install the built-in Servo library if not already available (Tools → Manage Libraries).
Select your board and port under Tools.
Upload the sketch.
Open Serial Monitor (9600 baud) to view live sensor readings and system state.
Notes
Mount the entry sensor far enough before the barrier to give it time to fully open before the vehicle arrives.
Mount the exit sensor just past the barrier's swing path, facing across the lane so a passing vehicle reliably breaks the beam.
No buzzer is used in this build — status is indicated only through barrier movement (and optionally, status LEDs).
License
Free to use and modify for personal or educational projects.
