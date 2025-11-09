# Project Lux: Circuit Walkthrough

This document provides a detailed, component-by-component explanation of the transmitter and receiver circuits.

## 🛰️ Transmitter Schematic

![Transmitter Schematic](../media/pictures/Transmitter_Schematic.png)
*(You should save your transmitter schematic in `media/pictures/` and update this link)*

The transmitter's function is to convert an audio **voltage** into a modulated **current** to drive a laser. This is a **transconductance** circuit.

### Block 1: Audio Input & DC Biasing
* **`J1` (AudioPlug2):** A 3.5mm audio jack. The 'T' (Tip) pin carries the left audio channel (or mono), which is used as our signal. The 'S' (Sleeve) is ground.
* **`C1` (1uF):** An **AC-coupling capacitor**. It blocks any DC voltage from the audio source, allowing only the AC audio signal (the "music") to pass through.
* **`R1`, `R2` (10k):** These form a **voltage divider**. They take the +5V supply and divide it exactly in half, creating a stable **2.5V DC bias**, or "virtual ground."
* **Function:** The AC signal from `C1` and the 2.5V DC bias from the resistors meet at Pin 3 of the op-amp. This "adds" the two, shifting the audio signal (which swings from ~-1V to +1V) to swing around 2.5V (i.e., from ~1.5V to ~3.5V). This is essential for our single-supply circuit, which cannot process negative voltages.

### Block 2: Amplification & Buffering
* **`U1A` (LM358):** The first op-amp. It's configured as a **non-inverting AC amplifier**.
* **`C2` (10uF) & `R4` (10k):** These components set the AC gain. The gain is $A_v = 1 + (R3 / R4) = 1 + (10k / 10k) = 2$.
* **`R3` (10k):** The feedback resistor. `C2` blocks DC, so for DC signals, the op-amp acts as a voltage follower (gain of 1), which just passes the 2.5V bias through.
* **Function:** This stage amplifies our audio signal by a factor of 2, making it stronger. The output at Pin 1 is a 2.5V DC bias with a twice-as-large AC signal "riding" on it.

### Block 3: The Transconductance Stage (Driver)
* **`R5` (1k):** A current-limiting resistor that connects the op-amp's voltage output to the transistor's "control" pin (the Base).
* **`Q1` (2N3904):** An NPN BJT transistor, which we are using as a **voltage-controlled current source**. It's the "valve" of the circuit. The voltage at its Base (Pin 2) controls how much current it allows to flow from its Collector (Pin 1) to its Emitter (Pin 3).
* **`R7` (39) & `R6_Variable`:** These resistors set the **quiescent (idle) current**. By adjusting `R_Variable`, you set the DC voltage at the emitter, which in turn sets the "baseline" current that flows through the transistor and laser *even when there is no audio*. This is critical for setting the laser to "half-brightness" so the audio signal can make it both brighter and dimmer.

### Block 4: Laser & Protection
* **`LD1` (D6505I):** The laser diode. The current flowing through `Q1` flows through `LD1`, and its brightness is directly proportional to this current.
* **`D1`:** A **flyback diode**. Laser diodes have some inductance and can create a negative voltage spike (flyback) when the current changes fast. This diode gives that spike a safe path to ground, protecting the transistor `Q1` from damage.

### Block 5: Power Supply Decoupling
* **`U1C` (LM358):** This is just the power-pin header for the LM358 chip. Pin 8 is +5V, Pin 4 is GND.
* **`C3` (0.1uF):** A **decoupling (or bypass) capacitor**. It's placed physically close to the op-amp's power pins and acts as a tiny, fast local battery. It filters out high-frequency noise from the power supply, ensuring the op-amp operates stably.

---

## 📡 Receiver Schematic

![Receiver Schematic](../media/pictures/Receiver-Schematic.png)
*(You should save your receiver schematic in `media/pictures/` and update this link)*

The receiver's function is to convert the modulated light **current** from a photodiode back into an audio **voltage**.

### Block 1: Photodetection & TIA Stage
* **`D1`, `D2`, `D3` (BPW34):** These are the photodiodes, connected in parallel to increase the light-sensitive area. When the laser hits them, they generate a small **current** proportional to the light's brightness.
* **`U2A` (LM358) & `R1` (10k):** This is the **Transimpedance Amplifier (TIA)**. Its job is to convert **current-to-voltage**.
* **Function:** The photodiodes dump their current into the op-amp's inverting input (Pin 2), which is a "virtual ground" (held at 0V). The op-amp generates a voltage at its output (Pin 1) to pull that same amount of current through the feedback resistor `R1`. The resulting output is $V_{out} = -I_{photodiode} \cdot R1$. This stage successfully converts the light-current into a voltage.

### Block 2: Biasing & Gain Stage
* **`R3` (5.1k) & `R5` (10k):** This is a **voltage divider** that creates a DC bias of $5V \cdot (10k / (5.1k + 10k)) \approx 3.3V$. This bias is fed to the non-inverting input (Pin 5) of the second op-amp.
* **`U2B` (LM358), `R4` (10k), `R2` (10k):** This is configured as an **inverting gain stage**.
* **Function:** This stage takes the signal from the TIA (fed into `R4`) and amplifies it with a gain of $A_v = -(R2 / R4) = -(10k / 10k) = -1$. The output signal at Pin 7 is centered around the 3.3V bias.

### Block 3: Power Amplifier & Filtering
* **`C2` (1uF):** An **AC-coupling capacitor**. It blocks the ~3.3V DC bias from the `U2B` stage, allowing only the clean, AC audio signal to pass to the power amplifier.
* **`U1` (PAM8403):** A 3W, Class-D stereo audio amplifier.
    * `INL` (Pin 7): The Left audio input, which receives our signal from `C2`.
    * `SHDN` (Pin 1) & `MUTE` (Pin 10): Shutdown and Mute pins. They are tied to the `VREF` pin, which (along with `C1`) sets them to an "enabled" state.
* **`C3` (20uF) & `C4` (0.1uF):** Power supply filtering and decoupling capacitors for the `PAM8403`. `C3` is the bulk capacitor to handle large current draws, and `C4` filters high-frequency noise.
* **`4S1` (speaker):** The 4-ohm speaker, connected to the Left differential output (`LOUT+` and `LOUT-`).
* **`U2C` (LM358):** The power pins (Pin 8 = +5V, Pin 4 = GND) for the LM358 op-amp chip.