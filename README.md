# Smart Parking Barrier System

An automated parking barrier that detects an approaching vehicle, raises the barrier arm, waits for the vehicle to pass through, then lowers the barrier automatically — no manual operation needed.

## Features

* Detects approaching vehicles using an ultrasonic entry sensor
* Automatically opens the barrier using a servo-driven arm
* Detects when the vehicle has fully passed using a second exit sensor
* Automatically closes the barrier once the lane is clear
* Serial Monitor debug output for live distance readings and system state

## Components Required

| Component                 | Quantity |
| ------------------------- | -------: |
| Arduino Uno/Nano          |        1 |
| HC-SR04 Ultrasonic Sensor |        2 |
| SG90 Servo Motor          |        1 |
| Breadboard                |        1 |
| Jumper Wires              |      ~15 |

## Circuit Connections

### Entry Sensor (HC-SR04)

| Pin  | Arduino |
| ---- | ------- |
| VCC  | 5V      |
| GND  | GND     |
| Trig | Pin 9   |
| Echo | Pin 8   |

### Exit Sensor (HC-SR04)

| Pin  | Arduino |
| ---- | ------- |
| VCC  | 5V      |
| GND  | GND     |
| Trig | Pin 7   |
| Echo | Pin 6   |

### Servo Motor

| Pin    | Arduino                    |
| ------ | -------------------------- |
| Signal | Pin 10                     |
| VCC    | 5V (or external 5V supply) |
| GND    | GND                        |

> **Note:** All VCC pins connect to the same 5V rail, and all GND pins connect to the same ground rail. If the servo jitters or causes Arduino resets, power it from a separate 5V source while still sharing a common ground with the Arduino.

## How It Works

1. **Idle** — The barrier stays closed while both sensors continuously check for objects.
2. **Vehicle detected at entry** — The entry sensor reads a distance below the threshold (default 15 cm), causing the barrier to open.
3. **Open & hold** — The barrier stays open for a short safety delay, giving the vehicle time to move under it.
4. **Waiting for clearance** — The system monitors the exit sensor. It must first detect the vehicle and then detect that the vehicle has moved away.
5. **Closing** — Once the vehicle has fully passed the exit sensor, the barrier lowers and the system returns to the idle state.

## Configuration

The following values can be adjusted at the top of the Arduino sketch to tune the system behavior:

```cpp
const int DETECT_THRESHOLD_CM = 15;       // Detection distance in cm
const int BARRIER_OPEN_ANGLE  = 90;       // Servo angle when open
const int BARRIER_CLOSED_ANGLE = 0;       // Servo angle when closed
const unsigned long BARRIER_OPEN_HOLD_MS = 3000; // Hold time after opening (ms)
```

## Setup Instructions

1. Wire the components according to the circuit connections described above.
2. Open `smart_parking_barrier.ino` in the Arduino IDE.
3. Install the built-in **Servo** library if it is not already available.
4. Select your Arduino board and port under **Tools**.
5. Upload the sketch to the Arduino.
6. Open the **Serial Monitor** and set the baud rate to **9600** to view live sensor readings and system state.

## Notes

* Mount the entry sensor far enough before the barrier to give it enough time to fully open before the vehicle arrives.
* Mount the exit sensor just past the barrier's swing path, facing across the lane so that a passing vehicle reliably triggers the sensor.
* No buzzer is used in this build. System status is indicated through barrier movement and can optionally be displayed using status LEDs.

## License

Free to use and modify for personal or educational projects.
