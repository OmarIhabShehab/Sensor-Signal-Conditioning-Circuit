# ⚡ Low-Level Sensor Signal Conditioning Circuit

<div align="center">

![License](https://img.shields.io/badge/License-MIT-blue?style=flat-square)
![Simulink](https://img.shields.io/badge/Tool-Simulink-orange?style=flat-square)
![Proteus](https://img.shields.io/badge/Tool-Proteus-red?style=flat-square)
![Course](https://img.shields.io/badge/Course-CIE%20201-brightgreen?style=flat-square)
![Status](https://img.shields.io/badge/Status-Complete-success?style=flat-square)

**A complete analog signal conditioning pipeline for a K-Type thermocouple — amplification, filtering, and scaling — designed, simulated in Simulink, and implemented on hardware.**

</div>

---

## 📁 Project Structure

```
Sensor-Signal-Conditioning/
│
├── Simulink/          # Simulink block diagram models
├── Hardware/          # Circuit photos & oscilloscope captures
├── Report/            # Phase 1 & Phase 2 reports (PDF)
└── README.md
```

---

## 📌 Abstract

This project designs a four-stage signal conditioning circuit for a **K-Type thermocouple** sensor producing signals in the microvolt range. The circuit amplifies, filters, and scales the raw sensor output to a clean **0–5 V** usable signal, verified through Simulink simulation and physical breadboard implementation.

---

## 🌡️ Sensor — K-Type Thermocouple

| Property | Value |
|---|---|
| Type | K-Type (Chromel–Alumel) |
| Sensitivity | 41 μV/°C |
| Temperature range | 0°C to 1024°C |
| Raw output | 0 μV to ~50 mV |
| Noise source | 50 Hz power-line interference (~0.3 mV) |
| Signal bandwidth | < 1 Hz (quasi-DC) |

---

## ⚙️ Circuit Architecture

### Signal Flow
```
K-Type Thermocouple
        ↓  (~1 mV DC + 50 Hz noise)
Instrumentation Amplifier (×1001)
        ↓  (~1 V)
2nd-Order Low-Pass Filter (fc = 1.6 Hz)
        ↓  (clean DC)
Output Scaling Amplifier (×5)
        ↓  (0–5 V)
Final Output
```

---

## 🔬 Stage-by-Stage Design

### Stage 1 — K-Type Thermocouple
- Seebeck effect: V = 41 μV/°C × ΔT
- At ΔT = 25°C → V_DC ≈ 1 mV
- Superimposed with 50 Hz noise (~0.3 mV amplitude)

### Stage 2 — Instrumentation Amplifier (MCP6002 × 2)

Two MCP6002 rail-to-rail op-amps in 3-op-amp IA configuration:

| Component | Value |
|---|---|
| R3 = R4 | 500 kΩ |
| R5 (gain resistor) | 1 kΩ |
| **Gain** | **Av = 1 + 2R3/R5 = 1001×** |
| Output | 1.025 mV × 1001 ≈ **1.026 V** |

- High CMRR rejects common-mode 50 Hz noise
- Amplifies differential thermocouple signal from μV → V range

### Stage 3 — 2nd-Order Butterworth Low-Pass Filter

**Transfer Function:**

H(s) = 101.0648 / (s² + 14.2172s + 101.0648)

| Parameter | Value |
|---|---|
| Cutoff frequency (fc) | 1.6 Hz |
| Angular cutoff (ωc) | 10.0531 rad/s |
| Damping ratio (D) | 0.707 (Butterworth) |
| Attenuation rate | 40 dB/decade |

**Two-stage passive RC implementation:**

| Stage | R | C | fc |
|---|---|---|---|
| Stage 1 | 1 kΩ | 100 μF | 1.59 Hz |
| Stage 2 | 10 kΩ | 1000 μF | 0.016 Hz |

Result: Complete rejection of 50 Hz interference ✅

### Stage 4 — Output Scaling Amplifier

Non-inverting amplifier configuration:

| Component | Value |
|---|---|
| Rf | 40 kΩ |
| Rin | 10 kΩ |
| **Gain** | **Av = 1 + Rf/Rin = 5×** |
| Output | 1.026 V × 5 ≈ **5 V** |

---

## 🧰 Component List

| # | Component | Specification | Qty |
|---|---|---|---|
| 1 | MCP6002 | Rail-to-rail Op-Amp | 3 |
| 2 | Resistors | 1 kΩ – 500 kΩ | Multiple |
| 3 | Capacitors | 100 μF & 1000 μF | 2 |
| 4 | Breadboard | 830 tie-point | 1 |
| 5 | Power Supply | 5 V regulated | 1 |

---

## 📊 Signal Levels Summary

| Stage | Input | Output |
|---|---|---|
| Thermocouple | Temperature difference | 1 mV + 50 Hz noise |
| Instrumentation Amp | 1 mV | ~1 V |
| Low-Pass Filter | Signal + noise | Clean DC |
| Scaling Amplifier | 1 V | **0–5 V** |

---

## 🛠️ Tools Used

| Tool | Purpose |
|---|---|
| Simulink | Block diagram design & signal simulation |
| Proteus | Circuit schematic design |
| Oscilloscope | Stage-by-stage hardware verification |
| Digital Multimeter | DC level measurement |

---

## 👥 Team

| Name | ID | Contribution |
|---|---|---|
| Mohammed Soliman | 202402280 | Sensor selection & analysis, Proteus design |
| Mohamed Tamer | 202400792 | Hardware implementation & testing |
| Omar Ihab Fared Abdo | 202401437 | Block diagram design, simulation, documentation |
| Ahmed Akef | 202401486 | Signal analysis, waveform verification, calculations |
| Karim Assem | 202400688 | Report writing, hardware testing, simulation |

Undergraduate project — Communications & Information Engineering Dept.
**University of Science and Technology, Zewail City**
CIE 201 — Advanced Electric Circuits | Spring 2026

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).
