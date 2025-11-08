# Project Lux: A Self-Aligning FSO Audio Link

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

A self-aligning, laser-based audio communication system built for the Howard University **IEEE H.O.P.E. (Hands-On PCB Engineering)** competition (Fall 2025).

![Project Lux Receiver Board 3D View](./media/Pictures/3D-View-Receiver-Board.png)

## 🎯 Project Overview

Project Lux is a free-space optical (FSO) communication system that transmits a high-fidelity audio signal over a laser beam. The key feature of this project is a closed-loop, self-aligning system that autonomously scans for, locks onto, and tracks a receiver. This creates a robust and user-friendly wireless audio link that is not susceptible to a broken link from minor movements.

### Key Features
* **Self-Aligning:** A dual-axis, servo-driven pan-tilt mechanism automatically aims the transmitter.
* **Audio Modulation:** An analog circuit modulates a 3.5mm audio input signal onto a laser diode.
* **Smart Receiver:** A quadrant photodiode array provides real-time positional feedback while simultaneously demodulating the audio signal.
* **Custom PCB:** All transmitter and receiver electronics are designed on custom PCBs using **KiCad**.

## ⚙️ Tech Stack & Components

### Hardware
* **Microcontroller:** Arduino Nano
* **Audio:** 3.5mm Audio Jacks, PAM8403D Audio Amplifier
* **Actuation:** 2x SG90 Servo Motors
* **Sensors:** Quadrant Photodiode Array
* **Circuits:** Op-Amps (e.g., LM358) for modulation/demodulation

### Software & Design
* **Firmware:** C++ (Arduino)
* **PCB Design:** KiCad (or Cadence)
* **Version Control:** Git & GitHub

## 📂 Repository Structure

This repository is organized to keep our hardware designs, firmware code, and documentation separate and clean.
```
/
├── hardware/
│   ├── Receiver board/     # KiCad project files for the receiver
│   └── Transmitter board/  # KiCad project files for the transmitter
├── firmware/
│   └── ...                 # Arduino C++ source code (coming soon)
├── media/
│   └── pictures
|         └─...                 # Schematics, 3D renders, and demo videos
├── docs/
│   └── ...                 # Datasheets, presentations, etc.
├── .gitignore
└── README.md
```
## 📖 Project Status

This project is **currently in progress**.

* [x] Phase 1: Research & Reference (Cornell ECE 4760)
* [x] Phase 2: Initial Circuit Design & Component Sourcing
* [x] Phase 3: Receiver PCB Layout
* [x] Phase 4: Transmitter PCB Layout
* [ ] Phase 5: Firmware Development (Scanning & Tracking)
* [ ] Phase 6: Final Integration & Testing

## 👥 Team

* Abhishek Rana - Hardware Design, Firmware Development
* Aayush Dahal - Hardware Design, Firmware Development
* Diego McCullough - Hardware Design, Firmware Development

## 📜 Reference & Inspiration

This project is heavily inspired by the "Laser Audio Transmitter" final project from Cornell University's ECE 4760 course.
* **Link:** [https://people.ece.cornell.edu/land/courses/ece4760/FinalProjects/s2009/dyz2_jl589/dyz2_jl589/index.html](https://people.ece.cornell.edu/land/courses/ece4760/FinalProjects/s2009/dyz2_jl589/dyz2_jl589/index.html)

## 📄 License

This project is licensed under the **MIT License**. See the `LICENSE` file for full details.
