# Project Lux: A Free-Space-Optical Audio Link

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

A laser-based audio communication system built for the Howard University **IEEE H.O.P.E. (Hands-On PCB Engineering)** Project (Fall 2025).

![Project Lux Receiver Board 3D View](./media/Pictures/3D-View-Receiver-Board.png)

## 🎯 Project Overview

Project Lux is a free-space optical (FSO) communication system that transmits a high-fidelity audio signal over a laser beam.

This project demonstrates the core principles of optical communication. The system consists of two main components:
1.  **The Transmitter:** Modulates an audio signal from a source (like a phone or laptop) onto a laser beam.
2.  **The Receiver:** Intercepts the laser beam, converts the light back into an electrical signal, and amplifies it to drive a speaker.

This creates a simple, point-to-point wireless audio link.

### Key Features
* **Audio Modulation:** An analog circuit modulates a 3.5mm audio input signal onto a laser diode.
* **Optical Detection:** A photodiode array (`BPW34`) detects the modulated laser beam and converts it back into an electrical signal.
* **Custom PCB:** All transmitter and receiver electronics are designed on custom PCBs using KiCad.

## 🛰️ Transmitter Board Design

The transmitter board is the "voice" of the system. Its job is to take a standard audio signal and convert it into a light-based signal by modulating the brightness of a laser.

### 1. Stage 1: Audio Input & Biasing
* **The Problem:** A standard audio (sine) wave is AC, meaning it has a positive and a negative "half." Our single-supply (+5V) circuit can't process negative voltages.
* **The Solution:** A **voltage divider** (`R1`, `R2`) creates a 2.5V "virtual ground." An **AC-coupling capacitor** (`C1`) lets the audio signal "ride" on top of this 2.5V bias, shifting the entire wave into the positive (1.5V to 3.5V) range.

### 2. Stage 2: Voltage-to-Current Conversion (Transconductance)
* **Modulation:** This is the core of the transmitter. It takes the *voltage* signal and converts it into a proportional *current* to drive the laser. This is a **transconductance** stage.
* **The Circuit:** An op-amp (`U1A`) buffers the signal, which then drives a transistor (`Q1`). The transistor acts as a "valve," controlling the current flowing through the laser.
* **Idle Current:** A variable resistor (`R6_Variable`) is used to set the "quiescent" or "idle" current. This keeps the laser on at "half-brightness" when no audio is playing, allowing the signal to modulate both brighter and dimmer.

### 3. Stage 3: Laser Driver
* **Output:** The modulated current from `Q1` drives the laser diode (`LD1`), causing its brightness to flicker in perfect sync with the audio waveform.

## 📡 Receiver Board Design

The receiver board is the "ear" of the system, designed to convert the incoming light signal back into audible sound. The signal path is a three-stage process.

### 1. Stage 1: TIA (Current-to-Voltage)
* **Detection:** An array of `BPW34` photodiodes captures the modulated laser light, generating a tiny electrical current proportional to the light's brightness.
* **Conversion:** The first op-amp (`U2A`) is configured as a **Transimpedance Amplifier (TIA)**. It converts this weak input *current* into a usable *voltage* signal.

### 2. Stage 2: Voltage Pre-Amplification
* **Gain:** The voltage from the TIA is fed into the second op-amp (`U2B`), which is configured as an **inverting gain stage** to boost the signal.

### 3. Stage 3: Power Amplification
* **Coupling:** A `1uF` capacitor (`C2`) blocks the DC component of the signal, passing only the clean AC audio to the final stage.
* **Power Amp:** A `PAM8403D` Class-D audio amplifier takes the line-level signal and provides the necessary power to drive a standard `4S1` speaker.

### Key Components
* **Photodiodes:** `BPW34` (x3)
* **Op-Amp (TIA & Pre-Amp):** `LM358`
* **Power Amplifier:** `PAM8403D`

> **Want more detail?**
>
> Check out the **[Full Circuit Walkthrough](./docs/Circuit-Walkthrough.md)** for a component-by-component explanation of the schematics.

## ⚙️ Tech Stack & Components

### Hardware
* **Audio:** 3.5mm Audio Jacks, PAM8403D Audio Amplifier, Laser Diode
* **Sensors:** BPW34 Photodiodes
* **Circuits:** Op-Amps (LM358), Transistors (2N3904), Passive Components

### Software & Design
* **PCB Design:** KiCad
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
│   └── Walkthrough.md               # Datasheets, presentations, etc.
├── .gitignore
└── README.md
```
## 📖 Project Status

This project is **currently in progress**.

* [x] Phase 1: Research & Reference (Cornell ECE 4760)
* [x] Phase 2: Initial Circuit Design & Component Sourcing
* [x] Phase 3: Receiver PCB Layout
* [x] Phase 4: Transmitter PCB Layout
* [ ] Phase 5: Final Integration & Testing

## 👥 Team

* Abhishek Rana - Hardware Design (Receiver Board, Transmitter Board)
* Aayush Dahal - Hardware Design (Transmitter Board)
* Diego McCullough - Hardware Design (Power Board)

## 📜 Reference & Inspiration

This project is heavily inspired by the "Laser Audio Transmitter" final project from Cornell University's ECE 4760 course.
* **Link:** [https://people.ece.cornell.edu/land/courses/ece4760/FinalProjects/s2009/dyz2_jl589/dyz2_jl589/index.html](https://people.ece.cornell.edu/land/courses/ece4760/FinalProjects/s2009/dyz2_jl589/dyz2_jl589/index.html)

## 📄 License

This project is licensed under the **MIT License**. See the `LICENSE` file for full details.
