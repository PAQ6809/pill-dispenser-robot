# Pill Dispenser Robot

Medication adherence is critical, especially for elderly or chronically ill patients. To help patients take the correct medicines at the prescribed times, this project provides an **automatic pill‑dispensing robot** with biometric verification and basic health monitoring.

## Features

- **Scheduled dispensing:** Uses a real‑time clock to dispense medicines at preset times. Three compartments correspond to morning, afternoon and night doses.
- **Rotating disk mechanism:** A stepper motor rotates a disk with precisely sized holes to release one pill at a time. Interchangeable disks support different pill sizes【635291607221944†L618-L624】.
- **Hand detection:** An ultrasonic sensor detects when a hand is present before dispensing【635291607221944†L595-L603】.
- **Biometric verification:** A fingerprint sensor (R307) verifies the user to prevent unauthorized access【635291607221944†L629-L637】.
- **Health monitoring:** Heart rate & oxygen saturation sensor (MAX30102), temperature sensor (DS18B20), and other sensors monitor basic vital signs【635291607221944†L629-L637】.
- **Notifications:** A SIM800L GSM module sends reminder messages to patients and caregivers【635291607221944†L603-L640】.
- **User interface:** Touchscreen or buttons allow users to set schedules, add/remove medicines and view health data【635291607221944†L603-L640】.

## System Architecture

The system is built around a **Raspberry Pi** microcontroller that controls stepper motors, reads sensor data and manages the schedule. Major components include:

| Module | Description |
|-------|-------------|
| **Pill dispenser** | A rotating disk driven by a stepper motor. Holes sized according to pill dimensions ensure one pill per rotation【635291607221944†L618-L624】. |
| **Sensors** | Ultrasonic sensor HC‑SR04 for hand detection; fingerprint sensor R307; MAX30102 for heart rate/SpO₂; DS18B20 for temperature【635291607221944†L595-L637】. |
| **Controller & software** | Raspberry Pi running Python scripts handles scheduling, sensor fusion and motor control【635291607221944†L629-L640】. |
| **Communications** | SIM800L GSM/GPRS module for SMS notifications; optional Wi‑Fi for integration with a smartphone app. |
| **Power & enclosure** | 5 V 5 A power supply; stepper motor drivers (e.g., L293D) and a compact housing for the compartments and electronics. |

## Getting Started

1. **Hardware assembly:** Connect the stepper motor to the disk, mount sensors on the housing and wire them to the Raspberry Pi. Use driver ICs (e.g., L293D) to power the motors【635291607221944†L650-L653】.
2. **Install dependencies:** Flash Raspberry Pi OS, enable SPI/I2C/UART interfaces, and install Python libraries such as `RPi.GPIO`, `pyserial` and sensor drivers.
3. **Configure schedule:** Edit the `schedule.json` file (to be created) to set dispensing times and compartments.
4. **Run the script:** Execute `python3 main.py` to start the dispenser. The program monitors the clock, verifies users and dispenses pills when scheduled.
5. **Receive alerts:** Register caregiver phone numbers to receive SMS notifications for missed doses or abnormal vital signs.

## Future Improvements

- Add a web or mobile app for remote schedule management and data visualization.
- Integrate machine learning to detect compliance patterns and provide personalized reminders.
- Design a more compact, modular enclosure for easier maintenance and refilling.

## References

This project design is inspired by research on smart medical boxes and automatic pill dispensers. For example, a study described a system using stepper motors to operate compartments, ultrasonic and biometric sensors for user detection, and health monitoring sensors connected to a Raspberry Pi【635291607221944†L595-L640】.
