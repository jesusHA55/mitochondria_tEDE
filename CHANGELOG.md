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
