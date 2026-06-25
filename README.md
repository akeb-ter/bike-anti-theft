# Bike Anti-Theft System (Arduino + MIT App Inventor)

An Arduino-based smart anti-theft system for bicycles. It pairs with an Android app (built via MIT App Inventor) over Bluetooth to provide real-time GPS tracking, motion detection alarms, and a physical servo-actuated brake lock.

## Features

* **Bluetooth Control:** Arm and disarm the bike remotely using an HC-05 module.
* **Motion Detection:** Triggers an alarm upon unauthorized movement or vibration.
* **GPS Tracking:** Reads coordinates from a GY-NEO6MV2 GPS module to track the bike's location.
* **Active Deterrents:** Activates a buzzer and an LED warning light when stolen.
* **Physical Brake Lock:** Engages a servo motor to lock the brakes when the alarm is triggered.
* **App Integration:** Streams real-time telemetry (State, Latitude, Longitude) in a clean CSV format tailored for MIT App Inventor.

## Hardware Requirements

* Arduino (Uno, Nano, or compatible)
* HC-05 Bluetooth Module
* GY-NEO6MV2 GPS Module
* Analog Vibration Sensor
* Servo Motor (for brake mechanism)
* Active Buzzer
* LED

## Wiring Configuration

Based on the primary `anti_theft.ino` firmware:

| Component | Arduino Pin | Notes |
| :--- | :--- | :--- |
| **HC-05 TX** | `RX (Pin 0)` | Hardware Serial |
| **HC-05 RX** | `TX (Pin 1)` | Hardware Serial |
| **GPS TX** | `D4` | SoftwareSerial RX |
| **GPS RX** | `D3` | SoftwareSerial TX |
| **Servo** | `D9` | PWM |
| **Buzzer** | `D8` | Digital Out |
| **LED** | `D7` | Digital Out |
| **Vibration Sensor** | `A0` | Analog In |

> **Note:** Disconnect the HC-05 module from Pins 0/1 when uploading sketches to the Arduino to prevent serial conflicts.

## Repository Structure

* `anti_theft.ino`: The main production firmware. Handles state management (Armed, Disarmed, Stolen), reads sensor data, and communicates strictly via CSV with the companion app.
* `debug.ino`: A diagnostic utility sketch. Use this during initial assembly to verify that each hardware component (Servo, GPS, Bluetooth, LED, Buzzer) is wired correctly and functioning. *Note: `debug.ino` uses different pinouts for testing purposes.*

## MIT App Inventor Integration

The Arduino communicates with the mobile app using a strict CSV format to ensure easy parsing in MIT App Inventor.

**Outgoing Data (Arduino -> App):**
The Arduino continuously broadcasts its state and location every 10 seconds, or immediately upon a state change.
Format: `STATUS,LATITUDE,LONGITUDE
`
*Example:* `ARMED,14.599512,120.984222`

**Incoming Commands (App -> Arduino):**
The app sends plain text string commands to control the system:
* `ARM` - Arms the system.
* `DISARM` - Disarms the system, stops the alarm, and releases the brake.
* `STATUS` - Forces an immediate status and location update.
