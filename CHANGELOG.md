# Changelog: ED-E Custom Motherboard (HAT)

> **Project:** ED-E Autonomous Companion Rover
> **Chassis:** Ikohs Netbot S12
> **Core Architecture:** Raspberry Pi 5 + Custom I2C/Power Distribution HAT
> **Format:** All notable changes to the PCB layout, schematic, and hardware BOM will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html) tailored for hardware revisions (Major.Minor.Patch).

---

## [Unreleased] - Work in Progress
### Routing & Layout
- TBD: Star grounding topology separating the LM2678 high-current returns from the PCA9685 logic grounds.
- TBD: Trace width calculations for the 5A/5V power polygons.
- TBD: Physical positioning of the 40-pin elevated female header to clear the Pi 5 Active Cooler.

---
## [0.1.3-alpha] - 2026-03-17
### H-Bridge Completion & I2C Sensor Payload

### Added
- **Completed Custom H-Bridge Architecture:** Finalized the dual full H-bridge design utilizing 4x `IR2104` half-bridge gate drivers and 8x low-Rds(on) N-channel MOSFETs. Implemented Sign-Magnitude PWM drive via the `SD` pins for smooth, efficient motor control.
- **Motor Output Connectors:** Upgraded from standard pin headers to 5.08mm pitch PCB screw terminal blocks (15A+ rated) to securely lock the heavy Duraframe track wires to the Medulla board and prevent vibration arcing.
- **GL-VSLAM IMU (I2C):** Integrated the Bosch `BNO085` 9-DOF System-in-Package (SiP). Selected specifically for its onboard Cortex-M0+ running MotionEngine sensor fusion, providing drift-free quaternions directly to the Pi 5 without I2C clock-stretching issues.
- **Total Pack Voltage Monitoring (I2C):** Integrated the Texas Instruments `ADS1115` 16-bit ADC (Address `0x48`) to monitor the 5S LiPo for safe software shutdowns.
- **Safety Voltage Divider:** Added a dedicated resistor voltage divider (68kΩ High-Side / 10kΩ Low-Side) to step the peak 21.5V battery voltage down to a safe ~2.75V for the ADS1115's A0 channel.
- **I2C Bus Stabilization:** Added 4.7kΩ pull-up resistors to the `SDA` and `SCL` traces, placed physically close to the PCA9685, to guarantee crisp data transmission through the electrical noise of the motors.

### Changed
- **I2C Topology:** Expanded the bus from a single device (PCA9685) to a multi-drop master/slave network including the BNO085 and ADS1115, all sharing Pi 5 Physical Pins 3 and 5.

## [0.1.2-alpha] - 2026-03-16
### Changed the internal names to Medulla
## [0.1.1-alpha] - 2026-03-16
### Added completed BMS + new subschematics
## [0.1.0-alpha] - 2026-03-16
### Initial Schematic Draft & Hardware Architecture

### Added
- **Main Power Plane:** Integrated Texas Instruments `LM2678-5.0 & LM2678-12.0` buck converter. Steps down the 5S LiPo battery (21.5V max) to a stable 5V at 5A to feed the Pi 5 directly through the GPIO.
- 
### To add
- **Gate Driver Power:** Integrated `LM7812` linear regulator to drop 21.5V to 12V specifically to supply the $V_{CC}$ pins on the motor gate drivers.
- **Motor Control Logic:** Added `PCA9685` 16-channel I2C PWM controller. Bypasses software PWM and limits Raspberry Pi pin usage strictly to SDA (Pin 3) and SCL (Pin 5). Powered directly via the Pi's 3.3V header.
- **Custom Motor Drivers:** Designed H-bridge schematic utilizing `IR2104` half-bridge gate drivers and low-Rds(on) N-channel MOSFETs for high-efficiency, low-heat motor control.
- **Odometry Traces:** Reserved 4 dedicated GPIO pins (GPIO 16, 20, 21)
