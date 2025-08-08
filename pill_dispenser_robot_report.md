# Pill‐Dispensing Robot Project Overview

This document summarises the design, implementation and lessons learned from a medication‑dispensing robot project.  The goal of the project was to develop an automated system that can detect a patient’s face and mouth, align a robotic arm and dispense a single pill at the correct moment.  The project also integrates health‑monitoring sensors and communications to caregivers.  A competition certificate and photographs of the prototype are included for context.

## Inspiration and Objectives

Traditional medication adherence relies on caregivers or patients to remember dosing times.  This can be problematic for people who are bedridden or have impaired memory.  The project aims to automate this process by using a **feeding robot** that acts as an extra pair of hands for patients.  The inspiration came from observing practical needs in healthcare: the system should automatically detect the user’s mouth and accurately dispense medication.  This requires the integration of **image recognition, mechanical control and communications**, making the project both technically challenging and practically valuable【635291607221944†L595-L640】.

## System Architecture

The system is divided into two main parts: a **Raspberry Pi** and an **Arduino** microcontroller【635291607221944†L595-L640】.

| Module | Function | Notes |
|---|---|---|
| **Raspberry Pi** | Captures real‑time images via a USB camera and runs OpenCV. It performs face and mouth detection using Haar Cascade classifiers and computes mouth coordinates \\((x, y)\). The Raspberry Pi sends these coordinates to the Arduino via UART. It also displays live video for testing and provides a user interface【635291607221944†L595-L640】. | Implemented in Python 3 with OpenCV. Uses `cv2.CascadeClassifier(cv2.data.haarcascades + 'haarcascade_frontalface_default.xml')` and `haarcascade_mcs_mouth.xml` for detection. |
| **Arduino** | Receives mouth coordinates and controls two MG995 servo motors. One motor moves the **mechanical arm** to align with the mouth; the other rotates a **transparent pill disk** to release a pill.  It sends status messages back via the serial connection. | Implemented in C++ (Arduino IDE). Correct pin assignments (arm on D9, disk on D10) are crucial【635291607221944†L650-L653】. |
| **Mechanical Components** | The robot includes a **circular pill disk** with holes sized to hold a single tablet.  The disk rotates to align the hole with a chute, ensuring that only one pill is dispensed at a time【635291607221944†L618-L624】.  An arm with a servo motor positions the disk over the patient’s mouth.  The base of the robot has **mecanum wheels**, allowing omnidirectional movement. | Servo angles and timing must be tuned (e.g., arm at 145 °, disk rotation of 100 °) to release pills smoothly【635291607221944†L650-L653】. |
| **Sensors & Communication** | Additional sensors such as a heart‑rate/SpO₂ module (MAX30102), temperature sensor (DS18B20) and fingerprint reader (R307) can monitor vital signs and verify the user’s identity【635291607221944†L595-L640】.  A GSM module (SIM800L) or Wi‑Fi can send SMS or cloud notifications to caregivers. | Future versions may integrate MQTT or mobile apps for remote monitoring【266709376490988†L1198-L1251】. |

## Development Challenges and Solutions

### Face & Mouth Detection

* **Camera Initialization and Colour Conversion:** Early tests failed because of incorrect imports and attempts to use PIL functions on OpenCV arrays.  The solution was to rely solely on OpenCV, using `cv2.VideoCapture` for camera input and `cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)` for grayscale conversion【635291607221944†L595-L640】.
* **Classifier Paths and Parameters:**  Using the wrong path to Haar Cascades or inappropriate `scaleFactor` and `minNeighbors` values led to unstable detection.  Setting the cascade path via `cv2.data.haarcascades` and tuning parameters to `scaleFactor = 1.1` and `minNeighbors = 3` improved stability【635291607221944†L595-L640】.  Constraining mouth detection within the face region and filtering detections to avoid false positives also helped【635291607221944†L595-L640】.

### UART Communication

* **Serial Port Selection:**  The initial code used `/dev/ttyUSB0`, but the actual serial port for the Arduino was `/dev/ttyUSB1`.  Adding error handling when opening the serial port makes the program exit gracefully if the Arduino is not connected【635291607221944†L595-L640】.
* **Transmission Errors:**  Without error checking, sending mouth coordinates could block the program.  Wrapping `ser.write()` in a `try/except` block and shortening the delay between transmissions (`time.sleep(0.3)`) made communication smoother【635291607221944†L595-L640】.

### Servo Control and Hardware Assembly

* **Incorrect Pin Numbers:**  The Arduino sketch initially attached the arm servo to pin 11; the correct pin was 9.  Incorrect pin assignments prevented the servo from moving【635291607221944†L650-L653】.  After correction, the arm servo responded as expected.
* **Power Supply Issues:**  Servos and motors draw more current than the microcontroller can supply.  A dedicated 5 V power source and proper grounding were needed to prevent resets【635291607221944†L650-L653】.
* **Disk Jamming:**  Pills occasionally jammed in the disk or chute.  Adjusting the rotation angle and tilting the chute by about 15 ° reduced friction and allowed pills to fall freely【635291607221944†L650-L653】.

### Movement and Mecanum Wheels

Testing revealed that one pair of mecanum wheels was wired backward.  Reversing the motor polarity and supplying 12 V improved mobility【635291607221944†L650-L653】.  Future work could develop a closed‑loop control strategy for the wheels to navigate hospital rooms.

## Testing and Lessons Learned

The robot was tested under different lighting conditions, distances and user positions.  Key findings include:

1. **Face Detection Sensitivity:** Haar Cascade classifiers work well in well‑lit environments but struggle under low light or with partial occlusion.  The team considered upgrading to a deep‑learning model such as MTCNN for better robustness【635291607221944†L595-L640】.
2. **Servo Calibration:**  The ideal angles for the arm and disk (≈145 ° and 100 °) were determined experimentally.  Reducing delays between operations improved the user experience but sometimes caused dropped serial messages; adding a buffer or handshake protocol could solve this.【635291607221944†L650-L653】
3. **User Feedback:**  Observers appreciated the system’s potential but pointed out that wiring needed tidying and detection should handle more varied poses and backgrounds.  Integrating an LED or buzzer to notify users of errors is planned.

## Future Improvements

The report suggests several avenues for enhancement:

1. **Deep‑Learning Face Detection:**  Replace Haar Cascades with a neural network (e.g., MTCNN) for more robust detection under different lighting and angles【635291607221944†L595-L640】.
2. **Distance Sensor Integration:**  Add ultrasonic or laser range sensors to fine‑tune arm positioning relative to the patient’s face.
3. **Improved Disk Design:**  Refine the pill disk and chute to ensure smooth dispensing, perhaps by reducing friction or adding a vibrating mechanism.
4. **Cable Management:**  Use cable sleeves or routing channels to reduce clutter and improve reliability.
5. **Mobility Control:**  Implement closed‑loop control for mecanum wheels to allow the robot to navigate to the bedside automatically.
6. **Error Indication:**  Integrate visual or auditory alerts (e.g., LEDs or buzzers) to signal misalignment, detection failures or empty pill compartments.
7. **Smartphone & Cloud Integration:**  Develop a mobile or web app that communicates with the robot via MQTT or REST APIs.  It can display schedules, logs and sensor data, and send notifications to caregivers【266709376490988†L1198-L1251】.

## Award and Prototype Images

The project was recognised with a second‑place award in the 2024 Taiwan Intelligent Robotics Competition (TIRC) creative design category.  The certificate highlights the achievement.

![Award Certificate]({{file:file-4N1tkHU1ZNs8F9Yukrs6QX}})

Photographs of the prototype show the robot’s omni‑wheel base, mechanical arm, pill disk and control electronics.  The first image below demonstrates the assembled robot without revealing any personal identity, and the top‑down view highlights the mechanical design.

![Pill‑dispensing robot prototype]({{file:file-FAVZ7SqMxd6KBoaM5r3whm}})

![Top view of the robot]({{file:file-XK4bqL8Ejv2o8QA4TPLs1B}})

These images emphasise the integration of mechanical, electrical and control components discussed in this report.  To respect privacy, individuals appearing in other photographs are not included.

## Conclusion

This project demonstrates how combining computer vision, mechatronics and communications can address real‑world healthcare challenges.  The team overcame issues related to code syntax, camera calibration, serial communication and mechanical alignment by consulting official documentation and community resources.  Future work includes deploying more robust face detection, enhancing mechanical design, improving mobility and adding connected features such as cloud data logging and caregiver notifications.  The insights documented here can guide others building similar medication‑dispensing robots.