# Pill Dispenser Robot

This repository contains an open-source medication dispensing robot. The project aims to help elderly or chronically ill patients adhere to medication schedules by automating the process of identifying the user, aligning a mechanical arm and dispensing a single pill at the right time. A Raspberry Pi performs face and mouth detection using OpenCV, while an Arduino controls servos to move the arm and rotate the pill disk.

## Features

- **Biometric verification:** A fingerprint sensor verifies authorised users and prevents misuse【635291607221944†L595-L640】.
- **Face & mouth detection:** A USB camera and OpenCV Haar Cascades detect the user’s face and mouth in real-time; coordinates are sent over UART to an Arduino【635291607221944†L595-L640】.
- **Motorised pill dispenser:** A servo-driven disk with precision holes releases exactly one pill per dose; interchangeable disks support different tablet sizes【635291607221944†L618-L624】.
- **Health monitoring:** Sensors (MAX30102 for heart rate and oxygen saturation, DS18B20 for temperature) collect vital signs and can notify caregivers【635291607221944†L595-L640】.
- **Notifications:** A SIM800L GSM module (or future MQTT/web integration) sends reminders and status updates【266709376490988†L1198-L1251】.
- **Modular design:** Separate modules for camera & processing, motion control, dispensing mechanism, sensors and communications make the system extensible.

## Repository structure

| Path | Purpose |
| --- | --- |
| `main.py` | Entry point script controlling scheduling, camera capture and serial communication |
| `pill_dispenser_robot_report.md` | Detailed report of the project design, hardware & software architecture, challenges and improvements |
| `.gitignore` | Excludes dependencies and temporary files |
| `LICENSE` | MIT license |

## Getting started

1. Clone this repository and install Python dependencies (OpenCV, numpy, pyserial) via `pip`.
2. Connect the Raspberry Pi camera or USB webcam, Arduino board, servos and sensors as described in the [detailed report](pill_dispenser_robot_report.md).
3. Configure the medication schedule by editing the list of `schedule_times` in `main.py`.
4. Run `python3 main.py` to start the system. The Raspberry Pi will detect faces/mouths, send coordinates to the Arduino and dispense pills accordingly.
5. To customise notifications or add a smartphone app, see the improvements section below.

## Future improvements

- Replace Haar Cascades with a deep-learning model (e.g., MTCNN) for more robust face detection【635291607221944†L595-L640】.
- Add ultrasonic or laser distance sensors to refine arm positioning.
- Improve the pill disk and chute design to prevent jams.
- Integrate mobile/web apps and cloud storage using MQTT or REST API【266709376490988†L1198-L1251】.
- Add error indicators (LEDs/buzzers) and better cable management for reliability.
- Implement closed-loop control for mecanum wheels for autonomous navigation.

## References

For a comprehensive description of the system, including motivation, hardware components, software architecture, debugging process and lessons learned, see the [project report](pill_dispenser_robot_report.md).