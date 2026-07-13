# 🛰️ Titan OS v13.0 (Autonomous RC Tank Edition)

An advanced, high-performance embedded control and tracking ecosystem tailored for the **Raspberry Pi 4B (2GB RAM)**. This platform blends dual-channel hardware remote control with real-time web telemetry, utilizing an array of structural fail-safes designed explicitly to protect **3D-printed mechanical drive trains**. 

Featuring a premium, dark flight cockpit interface, it enables long-range surveillance, automated waypoint navigation, and edge obstacle mitigation over standard networks.

---

## ⚡ Key Architecture Highlights

* **Gearbox Protection Matrix:** Implements digital **Anti-Jerk Slew Filters** that mathematically restrict aggressive acceleration updates per 20ms frame loop, keeping 3D-printed gears safe from structural fatigue or teeth stripping.
* **Dual-Plane Control Arbitration:** Actively processes high-frequency edge interrupts on the FlySky FS-i6X receiver (**Channel 5**). Flipping the physical toggle instantly overrides web commands to hand absolute control back to the manual sticks.
* **Terrain-Adaptive Gain Adjuster:** Continuously maps the tank's spatial position using an MPU gyro. The drive matrix automatically injects a throttle boost on steep climbs and scales back for dynamic braking on descents.
* **Proportional Kinematic Mixer:** Smoothly cross-fades from subtle curve adjustments (slowing down the inner track while boosting the outer track) into absolute zero-radius pivot turns depending on stick deflection magnitude.
* **Rohtak Region Geofence:** Features a localized coordinate anchor system tuned explicitly for high-accuracy tracking within the **Rohtak region, India**, using responsive satellite imagery layers that render structures in real time.

---


---
# 🛰️ UGV HORIZON 1 - COMPLETE DEPLOYMENT PLAYBOOK

This master deployment guide covers setting up your project files on your main computer, pushing them to GitHub, and pulling/running them directly on your Raspberry Pi 4B.


## 📁 1. ENVIRONMENT CONFIGURATION FILES

### File: requirements.txt
# Paste the following block directly into a file named "requirements.txt"
flask>=3.0.0
flask-socketio>=5.3.0
numpy>=1.24.0
opencv-python-headless>=4.7.0
smbus2>=0.4.2
pyserial>=3.5

---

### File: README.md
# Paste the following block directly into a file named "README.md"
# 🛰️ UGV HORIZON 1 (Autonomous RC Tank Edition)

🤖 Use Case: Autonomous long-range tactical surveillance rover. 💰 Build Cost: ~$220. ⚙️ Features: DJI-inspired HUD web dashboard, FlySky FS-i6X manual override, 3D-printed gear torque protection, terrain-adaptive IMU throttle, dual-sonar obstacle avoidance, NEO-6M GPS waypoint navigation, and live Pi camera video streaming.


## 🛠️ Master Hardware Pinout Reference

| Component | Function | Raspberry Pi BCM Pin | Physical Pin Header | Hardware Wiring Requirements |
| :--- | :--- | :--- | :--- | :--- |
| **Left Brushless ESC** | PWM Signal Line | GPIO 17 | **Pin 11** | Connect directly to Bidirectional ESC signal. |
| **Right Brushless ESC**| PWM Signal Line | GPIO 18 | **Pin 12** | Connect directly to Bidirectional ESC signal. |
| **Left Echo Sonar** | Trigger Pulse | GPIO 23 | **Pin 16** | Outbound ultrasonic transducer trigger. |
| **Left Echo Sonar** | Echo Input | GPIO 24 | **Pin 18** | **Must use 5V-to-3.3V logic level divider.** |
| **Right Echo Sonar** | Trigger Pulse | GPIO 22 | **Pin 15** | Outbound ultrasonic transducer trigger. |
| **Right Echo Sonar** | Echo Input | GPIO 27 | **Pin 13** | **Must use 5V-to-3.3V logic level divider.** |
| **FS-i6X Receiver** | Ch 1 (Steering) | GPIO 5 | **Pin 29** | Connect to iA6B Channel 1 PWM pin. |
| **FS-i6X Receiver** | Ch 2 (Throttle) | GPIO 6 | **Pin 31** | Connect to iA6B Channel 2 PWM pin. |
| **FS-i6X Receiver** | Ch 5 (Mode Switch)| GPIO 13 | **Pin 33** | Map to SwA or SwC on transmitter interface. |
| **MPU-9250 / 6050** | Serial Data (SDA) | GPIO 2 | **Pin 3** | I2C Telemetry Bus (Connect pull-up if missing).|
| **MPU-9250 / 6050** | Serial Clock (SCL)| GPIO 3 | **Pin 5** | I2C Telemetry Bus Clock Line. |
| **NEO-6M GPS** | UART Receive (RX) | GPIO 15 | **Pin 10** | Cross-connect to GPS Module **TXD** Pin. |
| **NEO-6M GPS** | UART Transmit (TX)| GPIO 14 | **Pin 8** | Cross-connect to GPS Module **RXD** Pin. |
| **System Power Ground**| Common Ground | GND | **Pin 6 / 14 / 20**| **All logic, ESCs, and RX must share this.** |

---

## 🚀 Quick-Start Deployment Guide

### 1. Initialize Host Dependencies
Before running the primary platform engine, updates and system-level input tracking daemons must be activated on your Raspberry Pi:
```bash
sudo apt-get update
sudo apt-get install pigpio python3-pigpio rpicam-apps -y
sudo systemctl enable pigpiod
sudo systemctl start pigpiod


<img width="477" height="615" alt="image" src="https://github.com/user-attachments/assets/022b3c67-f676-4dcc-a4ad-732d1d669fc2" />
<img width="490" height="722" alt="Screenshot 2026-07-13 090218" src="https://github.com/user-attachments/assets/ab2f758a-fede-42a8-8d11-39daad3be4d8" />
<img width="486" height="695" alt="Screenshot 2026-07-13 090307" src="https://github.com/user-attachments/assets/24df1cdf-8d65-4ac4-87a9-fa6cc2fd8239" />
<img width="466" height="469" alt="Screenshot 2026-07-13 090316" src="https://github.com/user-attachments/assets/b7240ba0-703a-4758-bb7a-53287b660ec2" />



