# Open/Short Test Theory

The system applies a stimulus (e.g. 0.2 V) through a known internal resistance (e.g. 20 Ω) and measures the voltage across the test object (RX). The resistance is computed from the divider:

$$R_X = \frac{20 \times V_{RX}}{0.2 - V_{RX}}$$

The measured voltage increases non-linearly with resistance, which is why the raw/short/open thresholds are chosen carefully.

## Purpose
- **Short test** — find *unexpected* shorts on the board.
- **Open test** — verify that components expected to be shorts (fuse, jumper, small resistor, inductor, trace, cable) are not open.

## Why thresholds matter
Expected shorts (jumpers, fuses, sub-threshold resistors/inductors) are learned as **short groups** so they are not flagged as defects. This saves program prep time and speeds up test.

# Short Groups

## How a short group is formed
During learning the tester starts from a nail and groups all nails whose impedance to it is below **Raw THD**. The group's maximum impedance must stay below **Open THD**.

### Example 1
Short THD = 5, Raw THD = 25, Open THD = 55.
Nails 1–6 = 22 Ω, 1–10 = 22 Ω, 10–4 = 30 Ω.
→ Groups: `[1, 6, 10]` and `[4]` (4–10 = 30 Ω > Raw THD).

### Example 2
10–4 = 22 Ω but 4–1 > 55 Ω → nail 4 stays out of the group.

### Example 3
Jumpers J1, J2, J3, inductor L1, large cap C1:
→ Groups: `[1,2]`, `[3,4]`, `[6,7,8]`. Large caps produce only *instant* shorts and are excluded. Nails 5, 9, 10 are signal nails.

## Raw THD tuning
Initial Raw THD = Short Threshold + 200 Ω. Lower it to reduce suspicious-short nails, or raise Open THD / manually add nails.

## Raw Test vs Fine Test
1. **Raw Test** — compare each group against all other nails; any impedance below Raw THD marks *suspicious short* nails.
2. **Fine Test** — test suspicious pairs individually against Short Threshold.

Sources of false suspicious shorts:
- **Instant short of capacitance** → fix with delay time (Test Nail/OPS parameter).
- **Phantom shorts** (parallel resistor paths) → fix by lowering Raw THD and re-learning.

## OPI and SPI
- **OPI (Open)** — every pair inside a short group becomes a test step; a higher-than-expected impedance = open.
- **SPI (Short)** — non-shorted pairs inside a group become test steps; deviations reveal opens/shorts.

# Jumper Test

Measured value conventions: **1 = Short**, **4 = Open**.

| Mode | Method | Threshold behavior |
|------|--------|--------------------|
| 0 | Adjust by OPS range | X<25 Ω → 1; 25–55 Ω → 3; >55 Ω → 4 |
| 1 | Resistor measure | X<5 Ω → 1; else 4 |
| 2 | Resistor measure | X<10 Ω → 1; else 4 |


# Resistor Measurement

## Constant current source
$$R_X = \frac{V_{RX}}{I}$$
Current scales with range (5 mA down to 0.1 µA) so the voltage stays in 0.15–1.5 V.

## Constant voltage source
Preferred when a large capacitor is in parallel — charging is fast, then $R_X = V_X / I_X$.

## Modes
| Mode | Description |
|------|-------------|
| 0 | Constant current |
| 1 | Low constant current (500 µA … 0.1 µA) |
| 2 | High-speed, 0.2 V DC source, $R_X = 0.2V/I_X$ |
| 3–5 | AC phase test (1 kHz / 10 kHz / 100 kHz) |
| 6 | 4-wire |
| 7 | 4-wire, faster (2 measurements), 64-nail grouping limit |
| 8 | 6-wire with guard nail (G-P3/G-P5) |

## Guarding
Shunt paths cause low readings. Driving the guard node to the same potential as the measured node (virtual ground via op-amp) blocks the shunt current.

## 4-wire formula (Mode 6)
$$R = \frac{R(\text{Hi},\text{Lo}) + R(G_1,G_2) - R(\text{Hi},G_1) - R(\text{Lo},G_2)}{2}$$

Mode 7 uses $R = V_m / I_m$ and measures twice instead of four times.

# Capacitor Measurement

## DC method
$$I \times T = C_X \times V_{CX}$$
Charge with constant current for fixed time T, then measure voltage.

## AC method
$$Z_C = \frac{1}{2\pi f C_X}$$
A fixed-frequency sine (40 mV RMS) is applied; current is measured and impedance derived.

## AC phase method
When CX is in parallel with RX, phase angle θ is needed:
$$C_X = C'_X \sin\theta$$
- RX → ∞ ⇒ θ → 90°
- RX → 0 ⇒ θ → 0°

## Modes
| Mode | Source |
|------|--------|
| 0 | 1 kHz |
| 1 | 10 kHz |
| 2 | 100 kHz |
| 3 | 1 MHz |
| 5/6/7 | 1 k / 10 k / 100 k phase |
| 4 | DC 5 mA (max 40000 µF) |
| 8 | DC 10 mA (best under 1000 µF) |
| 12 | 1 kHz phase, wide range (smaller MOA_R) |
| 14 | 100 kHz phase, wide range |
| 15 | AC 1 MHz, 4-wire (2 extra nails) |
| 11 | AC 1 kHz, 0.35 V SMD variant |

## Range table (excerpt)
| Capacitor | Mode 0 (1 kHz) | Mode 1 (10 kHz) | Mode 2 (100 kHz) | Mode 3 (1 MHz) |
|-----------|----------------|-----------------|------------------|----------------|
| 1 pF | 16 MΩ | 1.6 MΩ | 160 kΩ | — |
| 100 pF | 1.6 MΩ | 160 kΩ | 16 kΩ | 1.6 kΩ |
| 10 nF | 16 kΩ | 1.6 kΩ | 160 Ω | 16 Ω |
| 1 µF | 160 Ω | 16 Ω | — | — |

# Inductor Measurement

## Theory
$$Z_L = 2\pi f L_X$$
Fixed-frequency sine (40 mV RMS) is applied; current measured, impedance derived. Phase angle separates LX from parallel RX:
$$|Y'| \cos\theta = Y_{RX} = \frac{1}{R_X}$$

DC cannot be used — inductor is open initially, short when stable.

## Modes
| Mode | Frequency |
|------|-----------|
| 0 | 1 kHz |
| 1 | 10 kHz |
| 2 | 100 kHz |
| 3 | 1 MHz |
| 9 | 100 Hz |
| 5/6/7 | Phase variants (1 k / 10 k / 100 k) |

## Range table (excerpt)
| Inductor | 1 kHz | 10 kHz | 100 kHz | 1 MHz |
|----------|-------|--------|---------|-------|
| 10 µH | 63 mΩ | 0.63 Ω | 6.3 Ω | 63 Ω |
| 1 mH | 6.3 Ω | 63 Ω | 630 Ω | 6.3 kΩ |
| 1 H | 6.3 kΩ | 63 kΩ | 630 kΩ | 6.3 MΩ |

# Transistor Measurement

## Concept
Emitter grounded; raise V_BE until the transistor saturates (V_CE < 0.2 V).

## VCE modes
| Mode | Type | Gate voltage | Notes |
|------|------|--------------|-------|
| 3 | PNP | 0 to −12 V, 1 kΩ series | C via 200 Ω to +2 V |
| 4 | NPN | 0 to +12 V, 1 kΩ series | C via 200 Ω to +2 V |

Test sequence: diode junction (BC, BE) → apply gate voltage → expect V_CE ≈ 0.2 V.

## Hfe modes
| Mode | Type | Sense R | Method |
|------|------|---------|--------|
| 12 | NPN | 200 Ω | Hfe = Ic/Ib |
| 13 | PNP | 200 Ω | Hfe = Ic/Ib |
| 32 | NPN | 20 Ω | Hfe = Ic/Ib |
| 33 | PNP | 20 Ω | Hfe = Ic/Ib |
| 34 | NPN | 200 Ω | ΔIc/ΔIb, 2-phase |
| 35 | PNP | 200 Ω | ΔIc/ΔIb, 2-phase |
| 36 | NPN | 20 Ω | ΔIc/ΔIb, 2-phase |
| 37 | PNP | 20 Ω | ΔIc/ΔIb, 2-phase |

For modes 34–37, second gate voltage = BOM-V + 10/Expect-V; Hfe = (Ic'−Ic)/(Ib'−Ib).

# FET Measurement

## Test mode table
| Type | Mode | BOM-V | Expect | Function |
|------|------|-------|--------|----------|
| QF | 14 | 0 to +12 V | Id current | N-MOSFET |
| QF | 15 | 0 to −12 V | Id current | P-MOSFET |
| QF | 18 | 0 to −12 V | Id current | N-JFET |
| QF | 19 | 0 to +12 V | Id current | P-JFET |
| QF | 20 | 0 to −12 V | Vds | N-JFET, RD=20 |
| QF | 21 | 0 to +12 V | Vds | P-JFET, RD=20 |
| QF | 28 | 0 to +12 V | Rds | N-FET, RA=20 |
| QF | 29 | 0 to −12 V | Rds | P-FET, RA=20 |
| QF | 30 | 0 to +12 V | Rds | N-FET, RA=200 |
| QF | 31 | 0 to −12 V | Rds | P-FET, RA=200 |
| QB | 26 | Gate 0 to +12 V | Vds | P-type, 2 adjustable sources |
| QB | 27 | Gate 0 to −12 V | Vds | N-type, 2 adjustable sources |

Rds computed as: `Rds = 20 × Vds / (2 − Vds)` (RA = 20 Ω).

To verify turn-on, increase gate voltage until Id ≈ 10 mA.


# Clamping Diode Test

ICs with clamp diodes have a diode from I/O to VCC and GND. Force 3 mA (voltage limit 2.75 V) between I/O and VCC; if the pin or VCC is open, the diode cannot clamp near 0.7 V and the measured voltage exceeds the clamp threshold.

Symmetrically, force current GND → I/O to test GND-side opens.

# Agilent TestJet Technology

## Principle
A sensor plate under the IC forms a small capacitor with the lead frame. A 300 mV, 10 kHz signal is driven from the test pin; when the pin is connected, the sensor amplifier sees a larger signal than when the pin is open (no lead-frame coupling).

Typical values:
- Cx > 20 fF when normal
- Cx ≈ 0 fF when open

$$1/C_T = 1/C_x + 1/C_y$$

No physical probe needed on the pin itself — purely capacitive coupling.

# 3-Way Capacitor Polarity Test

## Requirements
- MDA board ≥ 518FR-002-8 (or U64 EPLD updated)
- Software ≥ V1.03B
- Vertical electrolytic capacitors only

## Theory
For vertical electrolytics the negative pole to case impedance is much lower than positive to case. Apply 0.2 V across +/− and measure case voltage (G-P1 sense):
- **Vo1** normal
- **Vo2** reversed (Vo2 << Vo1)
- **Vo3** missing part (≈ 0 V)

Because R1 << R2, comparison of Vo1/Vo2/Vo3 is a reliable pass/fail.

## Test program fields
| Field | Value |
|-------|-------|
| Act_V | 0.2 V |
| Std_V | Debug-derived threshold |
| Hlim | −1 (don't care) |
| Llim | ≈ 20 |
| Mode | 8 (or 18 for anti-burst caps) |
| Type | PX |
| Hip | Capacitor negative |
| Lop | Capacitor positive |
| G-P1 | Sense pin (case) |

## Debug notes
- Same-value caps share Hip/Lop (e.g. CE1–CE3 → 1, 3).
- Missing part reads −1; reverse mode 18 is negative; mode 8 below `Threshold × (1 − Llim%)`.
- Swap Hip/Lop to determine best Std_V; if both are very low check the third-end contact.

# Diode Test

## Modes
| Type | Mode | BOM-V | Expect | Function |
|------|------|-------|--------|----------|
| D | 0 | forward knee | 10 V–3 mA | forward bias |
| D | 1 | forward knee | 10 V–20 mA | forward bias |
| D | 5 | voltage source | expected current | NPF, 12 V max, R=1 kΩ |
| D | 6 | voltage source | expected current | NPF, 12 V max, R=200 Ω |
| D | 24 | forward knee | 10 V–1 mA | forward bias |
| D | 25 | forward knee | 10 V–5 mA | forward bias |

## Forward knee test
Voltage-limited current source forward-biases the diode; DVM measures forward voltage.

Voltage limit lookup:
| Expect-V | Voltage limit |
|----------|---------------|
| ≤ 2 | 1.35 + X |
| 2–5 | 2.5 + X |
| 5–X | 3.5 + X |

## Type D polarity (Modes 5, 6)
Apply voltage to the polarized cap; measure leakage current. Correct polarity (+) → P pin yields near-zero current. Limits: 10 V max, 3 mA (mode 5), 30 mA (mode 6).

## Type HV (Zener, Modes 5, 6)
Apply 0–48 V through Rs (10 kΩ mode 5, 1 kΩ mode 6), measure breakdown voltage. Max currents 48 mA (mode 5), 4.8 mA (mode 6). If breakdown > system range, only forward-bias testing is possible.

# SCR Test

## Mode 16
Programmable 0–10 V source.
- Hi-P = K, Lo-P = A, G-P1 = G
- Type **QS**, unit mA

## Two-point method
Detect SCR orientation via the Gate–Cathode diode.

## Three-point method
Apply two programmable sources (≤ 5 V) to G–K and A–K, measure Anode current.

## Tips
- Run single-step to confirm, then full-page/block tests for stability.
- If measured << expected: add delay or repeat.
- If measured >> expected: verify pin positions, Stand_V, part presence.
- Add "D" in the RPT field to repeat.

# Photo Coupler Test

## Mode 7
- 2 V fixed to Collector via 200 Ω
- BOM-V applied to diode via 200 Ω
- Hi-P = Collector, Lo-P = Emitter, G-P1 = Anode, G-P2 = Cathode
- Type **PC**, unit V

## Diode method
Check the Anode–Cathode diode for orientation.

## Four-terminal method
Apply two programmable sources to Anode and Collector; measure Vce(ON).

## Debug checklist
- Verify pin assignments (HiN/LoN/G-P1/G-P2).
- Ensure Stand_V is enough to turn the diode on.
- Confirm the part is present and not defective.


