# Medulla: The Autonomic Controller HAT

**Project:** tED-E (Autonomous Companion Rover)  
**Chassis:** Duraframe (Modified Ikohs Netbot S12)  
**Brain:** Raspberry Pi 5  

The Medulla is a custom-designed Hardware Attached on Top (HAT) for the Raspberry Pi 5. It acts as the autonomic nervous system for the tED-E companion rover, managing high-current power distribution and low-level motor reflexes. By offloading hardware-level PWM generation and power regulation, the Medulla allows the Pi 5 to dedicate 100% of its CPU resources to high-level cognitive tasks: initially AruCo marker tracking, and eventually full RGB-D GL-VSLAM mapping.

---

## ⚡ Power Architecture
The Duraframe chassis is powered by a 5S LiPo battery (21.5V Peak). The Medulla takes this raw voltage and strictly divides it to protect the sensitive logic components while maximizing motor efficiency.

* **Raw Battery (21.5V Max):** Routed directly to the N-channel MOSFET H-bridges to deliver maximum torque to the Duraframe tracks.
* **Cognitive Power Rail (5.0V / 5A):** Regulated by an onboard Texas Instruments `LM2678-5.0` buck converter. This feeds the Raspberry Pi 5 directly through the GPIO header (Pins 2 & 4), entirely bypassing the USB-C 3A limit to prevent CPU throttling during intense SLAM matrix calculations.
* **Reflex Power Rail (12.0V):** Regulated by an `LM7812` linear regulator to supply the $V_{CC}$ of the `IR2104` gate drivers, enabling the bootstrap capacitors to fully open the high-side MOSFETs without punching through their silicon limits.

---

## 🧠 Motor Logic & Control
To eliminate motor twitching and CPU-bound timing issues, the Pi 5 does not directly drive the motors.

* **The Middleman (`PCA9685`):** A 16-channel, 12-bit PWM controller sits on the I2C bus. The Pi 5 sends simple hexadecimal commands (e.g., "Set speed to 60%"), and the PCA9685 physically holds that electrical state flawlessly. 
* **The Muscles (`IR2104` + MOSFETs):** The PCA9685 triggers the gate drivers, which handle the heavy switching of the custom high-efficiency N-channel H-bridge.

---

## 📍 Pinout & Pi 5 Interface
The Medulla interfaces with the Raspberry Pi 5 using an elevated 2x20 female header (11mm+ clearance) to physically clear the Pi 5 Active Cooler. To preserve pins for future audio and sensor expansion, the board only utilizes **7 core pins** for power and motor control.

| Physical Pin | Function | Routing | Description |
| :--- | :--- | :--- | :--- |
| **2** | `5V IN` | LM2678 Output | Load-sharing 2.5A from the buck converter. |
| **4** | `5V IN` | LM2678 Output | Load-sharing 2.5A from the buck converter. |
| **6** | `GND` | Star Ground Plane | High-capacity ground return. |
| **9** | `GND` | Star Ground Plane | High-capacity ground return. |
| **1** | `3.3V OUT` | PCA9685 $V_{DD}$ | Powers the I2C logic chip. |
| **3** | `SDA` (GPIO 2) | I2C Bus | Data line to PCA9685 (and future IMU). |
| **5** | `SCL` (GPIO 3) | I2C Bus | Clock line to PCA9685. |

### Reserved Interrupt Pins (Odometry)
For accurate GL-VSLAM map scaling, the following pins are reserved at the bottom of the header for the Duraframe's quadrature wheel encoders:
* **GPIO 16 (Pin 36):** Left Motor Channel A
* **GPIO 20 (Pin 38):** Left Motor Channel B
* **GPIO 21 (Pin 40):** Right Motor Channel A
* **GPIO 26 (Pin 37):** Right Motor Channel B

---

## 🛠️ Build Notes
* **Grounding:** The PCB utilizes a strict star-grounding topology. The high-current switching returns from the LM2678 and the motor drivers are physically isolated from the delicate I2C logic grounds, meeting only at the negative battery terminal to prevent ground-bounce from resetting the Pi.
