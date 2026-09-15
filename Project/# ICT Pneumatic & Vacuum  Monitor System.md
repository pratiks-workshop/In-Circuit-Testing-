## Problem Statement
During high-volume PCB testing on TRI ICT machines, vacuum drops or air pressure fluctuations often go unnoticed by operators. When vacuum pressure drops below the required threshold:
* **Fixtures lose pull-down force:** Pogo pins fail to make proper contact with test pads.
* **False Failures (False Calls):** Good Devices Under Test (DUTs) are flagged as failed due to open circuit errors.
* **Component/Pin Damage:** Incomplete seal pressure can lead to misaligned pin contact and trace wear.

---

## Solution Overview
This system implements a hardwired hardware safety interlock using **SMC Digital Pressure Switches** operating in **Window Comparator Mode** connected directly to **24V DC switching relays**. 

No PLC or microcontroller is required, simplifying deployment and avoiding software failure points.

```text
[ Positive Air Line ] ---> [ SMC ISE20 Switch ] ---> [ Relay CR1 ]
                                                          | (NO Contact in Series)
[ Vacuum Line ]       ---> [ SMC ZSE20 Switch ] ---> [ Relay CR2 ]
                                                          |
                                       +------------------+------------------+
                                       |                                     |
                           [ ICT Start Enable Signal ]             [ Red Light / Buzzer ]
                            (Passes ONLY when OK)                   (Failsafe Alert)
```
![image](https://github.com/pratiks-workshop/In-Circuit-Testing-/blob/main/Project/TRIAirVacuum%20Monitor.png?raw=true "Optional Title")
### 3.3 Operational Specifications

| Parameter              | Sensor       | Window Range                                              | Signal Output Logic              |
| :--------------------- | :----------- | :-------------------------------------------------------- | :------------------------------- |
| **Main Air Supply**    | SMC ISE20    | 4.0 to 6.0 kg/cm² (4 to 6 bar)                            | Active-HIGH (24 V) when in window |
| **Fixture Vacuum**     | SMC ZSE20    | −16 to −25 cmHg (approx. −20 to −33 kPa)                  | Active-HIGH (24 V) when in window |

> **Note:** Both sensors output **24 V (Active-HIGH)** only while their input
> remains inside the specified window. If either reading drifts outside its
> range, the corresponding relay (CR1 / CR2) drops out, the series interlock
> opens, and the ICT start enable is blocked.

---

## SMC ISE20 & ZSE20 Sensor Technical Details
### SMC ISE20 — Positive Pressure Switch
**Application:** Monitors main air supply pressure (4.0–6.0 kg/cm²) for fixture
actuation.

#### Input Specifications

| Parameter                | Specification                                      |
| :----------------------- | :------------------------------------------------- |
| Power Supply             | 12–24 V DC ±10% (ripple p-p 10% max)               |
| Current Consumption      | ≤ 25 mA                                            |
| Applicable Fluid         | Air, non-corrosive / non-flammable gas             |
| Rated Pressure Range     | −0.100 to 1.000 MPa (positive pressure)            |

#### Output Specifications

| Parameter                  | Specification                                      |
| :------------------------- | :------------------------------------------------- |
| Output Type                | NPN or PNP open collector, 1 output                |
| Output Mode                | Hysteresis, window comparator, error output, or OFF |
| Max Load Current           | 80 mA                                              |
| Max Applied Voltage (NPN)  | 28 V                                               |
| Internal Voltage Drop      | ≤ 1 V (at 80 mA load)                              |
| Response Time (Delay)      | ≤ 1.5 ms (anti-chatter: 20–5,000 ms selectable)    |
| Short-Circuit Protection   | Yes                                                |

#### Operating Principle

1. **Sensing Element**
   Piezoresistive silicon diaphragm converts pressure into a differential
   voltage.

2. **Signal Conditioning**
   Internal ASIC amplifies and linearizes the signal; the 3-screen LCD displays
   the instantaneous value (red/green) and setpoints (orange).

3. **Comparator Logic**
   Two thresholds are configured:
   - **Setpoint (n_1):** Pressure at which output turns ON.
   - **Hysteresis (H_1):** Pressure at which output turns OFF.

4. **Output Activation**
   When measured pressure crosses the setpoint threshold, the open-collector
   output switches (sources +24 V for PNP), energizing relay CR1.

---
### 4.2 SMC ZSE20 — Vacuum Pressure Switch

**Application:** Monitors fixture vacuum level (−16 to −25 cmHg) for part
clamping verification.
#### Input Specifications

| Parameter                | Specification                                      |
| :----------------------- | :------------------------------------------------- |
| Power Supply             | 12–24 V DC ±10%                                    |
| Current Consumption      | ≤ 25 mA (ZSE20); ≤ 35 mA (ZSE20B)                  |
| Applicable Fluid         | Air, non-corrosive / non-flammable gas             |
| Rated Pressure Range     | 0.0 to −101.0 kPa (vacuum)                         |

#### Output Specifications

| Parameter                  | Specification                                      |
| :------------------------- | :------------------------------------------------- |
| Output Type                | NPN or PNP open collector, 1 output (ZSE20); 2 outputs + analog (ZSE20B) |
| Output Mode                | Hysteresis, window comparator, error output, or OFF |
| Max Load Current           | 80 mA                                              |
| Max Applied Voltage (NPN)  | 28 V                                               |
| Internal Voltage Drop      | ≤ 1 V (at 80 mA load)                              |
| Response Time (Delay)      | ≤ 1.5 ms                                           |

#### Operating Principle

1. **Sensing Element**
   Piezoresistive silicon diaphragm referenced to atmosphere; vacuum pulls the
   diaphragm negative relative to ambient.

2. **Signal Conditioning**
   Internal ASIC converts the differential to a digital value; the 3-screen
   display shows vacuum in the selected unit (kPa, cmHg, etc.).

3. **Comparator Logic**
   Two thresholds are configured:
   - **Setpoint (P_1):** Vacuum level at which output turns ON.
   - **Hysteresis (H_1):** Differential to prevent chatter.

4. **Output Activation**
   When vacuum reaches the setpoint, the open-collector output switches
   (sources +24 V for PNP), energizing relay CR2.
