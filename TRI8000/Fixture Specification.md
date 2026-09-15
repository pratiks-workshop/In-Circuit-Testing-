# 1. Introduction & System Architecture

The Test Research Inc. (TRI) TR8000 series smart in-circuit test (ICT) system
family comprises high-performance, high-throughput manufacturing test solutions.
The main models covered under this fixture specification standard are:

- TR8001
- TR8100LV
- TR8001A
- TR8100ALV

## 1.1 Model Nomenclature & Functional Differences

| Feature | Description |
| :------ | :---------- |
| **MDA Function Relay Check** | Systems **without** the `-LV` suffix support Manufacturing Defects Analyzer (MDA) Function Relay Checks. Models designated with `-LV` (e.g., TR8100LV, TR8100ALV) do **not** support the MDA Function Relay Check feature. |
| **Analog Switching Capabilities** | Models incorporating `-A` in their naming convention (e.g., TR8001A, TR8100ALV) feature one or more high-speed analog switch boards for precision measurement routing. |
| **General Abbreviation** | Across all technical documentation and fixture layout references, "TR8000" refers collectively to all system variants unless explicitly stated otherwise. |

## 1.2 Core Architecture Components

Every TR8000 test system installation consists of three primary functional
hardware modules:

1. **Tester Main Cabinet**
   The primary enclosure containing main AC/DC power supplies, vacuum manifold
   system, vacuum-driven test bed interface, system control electronics, and
   card cages housing the Testing/Switching Boards.

2. **Testing Boards**
   Function-specific cards (e.g., Switching Boards, System Management Board,
   On-Board Programming boards, Counter Buffer cards) installed inside the main
   cabinet to execute in-circuit test routines.

3. **Stand-alone Host PC**
   The system controller running the TR8000 System Testing and Diagnostics
   software environment. Handles operator interactions, test execution logs,
   and diagnostic routines.

---

# 2. Mechanical Specifications & Vacuum Interface

## 2.1 Mechanical Dimensions & Tolerances

| Parameter / Feature            | Dimension (mm) | Description / Mechanical Purpose                                      |
| :----------------------------- | -------------: | :-------------------------------------------------------------------- |
| Outer Base Width               |         680.00 | Full horizontal footprint of the fixture enclosure base.              |
| Middle Frame Width             |         550.00 | Outer rim frame guide width.                                          |
| Active Probe Bed Width         |         520.00 | Maximum usable horizontal width for probe pin population.             |
| Total Fixture Depth            |         450.00 | Overall depth from front lip to rear interface.                       |
| Fixture Base Height            |         150.00 | Bottom enclosure height housing internal wiring & interface blocks.   |
| Total Fixture Height           |         261.50 | Height from fixture base to the top press-plate handles.              |
| Upper Vacuum Plate Profile     |          65.10 | Height of the top vacuum sealing chamber/hood.                        |
| Interface Pin Clearance        |          15.00 | Bottom interface pin travel clearance.                                |
| Under-Fixture Clearance        |          70.00 | Clearance allowance for lower mechanical actuators/supports.          |
| Press Stroke Clearance         |          51.00 | Vertical mechanical stroke distance during vacuum pull-down.          |

# 3. Power Supply Interface Specifications

## 3.1 Standard Power Supply Interface Table (CN33 / CN34)

The main power supply interface provides regulated DC supply lines and sensing
paths to power the fixture, relays, and DUT.

| Pin | Line Name  | Function / Description              | Pin | Line Name  | Function / Description              |
| --: | :--------- | :---------------------------------- | --: | :--------- | :---------------------------------- |
|  1  | UGND-SNS   | Ground Sense Return Line            |  2  | UGND       | System Ground (NC)                  |
|  3  | UGND       | System Digital Ground Line          |  4  | UGND       | System Digital Ground Line          |
|  5  | UGND       | System Digital Ground Line          |  6  | UGND       | System Ground (NC)                  |
|  7  | UGND       | System Digital Ground Line          |  8  | UGND       | System Digital Ground Line          |
|  9  | UGND       | System Digital Ground Line          | 10  | UGND       | System Digital Ground Line          |
| 11  | UGND       | System Digital Ground Line          | 12  | UGND       | System Digital Ground Line          |
| 13  | 5VaSense   | +5 V Analog Remote Sense            | 14  | 5Va        | +5 V Analog Power Rail              |
| 15  | 5Va        | +5 V Analog Power Rail              | 16  | 5Va        | +5 V Analog Power Rail              |
| 17  | 5Vb        | +5 V Power Rail B                   | 18  | 5Vb        | +5 V Power Rail B                   |
| 19  | 5Vb        | +5 V Power Rail B                   | 20  | 5Vb        | +5 V Power Rail B                   |
| 21  | 5Vc        | +5 V Power Rail C                   | 22  | 5Vc        | +5 V Power Rail C (NC)              |
| 23  | 5Vc        | +5 V Power Rail C                   | 24  | 5Vc        | +5 V Power Rail C                   |
| 25  | 3.3VaSense | +3.3 V Analog Remote Sense          | 26  | 3.3Va      | +3.3 V Power Rail A                 |
| 27  | 3.3Va      | +3.3 V Power Rail A                 | 28  | 3.3Va      | +3.3 V Power Rail A                 |
| 29  | 3.3Vb      | +3.3 V Power Rail B                 | 30  | 3.3Vb      | +3.3 V Power Rail B                 |
| 31  | 3.3Vc      | +3.3 V Power Rail C                 | 32  | 3.3Vc      | +3.3 V Power Rail C                 |
| 33  | 12VaSense  | +12 V Remote Sense Line             | 34  | 12Va       | +12 V Power Rail A                  |
| 35  | 12Vb       | +12 V Power Rail B                  | 36  | 12Vb       | +12 V Power Rail B                  |
| 37  | NEG12V     | −12 V Auxiliary Supply              | 38  | NEG12V     | −12 V Auxiliary Supply (NC)         |
| 39  | ADJV SNS   | Adj. Voltage Remote Sense           | 40  | ADJV       | Adjustable Voltage Output           |
| 41  | ADJV       | Adjustable Voltage Output           | 42  | ADJV       | Adjustable Voltage Output           |
| 43  | UGND       | System Digital Ground Line          | 44  | UGND       | System Digital Ground Line          |
| 45  | UGND       | System Digital Ground Line          | 46  | UGND       | System Digital Ground Line          |
| 47  | PV1Sense   | Programmable Voltage 1 Sense        | 48  | PV1        | Programmable Voltage Supply 1       |
| 49  | PV1        | Programmable Voltage Supply 1       | 50  | PV1        | Programmable Voltage Supply 1       |
| 51  | PV1        | Programmable Voltage Supply 1       | 52  | PV1        | Programmable Voltage Supply 1       |
| 53  | PV1        | Programmable Voltage Supply 1       | 54  | PV1        | Programmable Voltage Supply 1 (NC)  |
| 55  | PV2        | Programmable Voltage Supply 2       | 56  | PV2        | Programmable Voltage Supply 2       |
| 57  | PV2        | Programmable Voltage Supply 2       | 58  | PV2        | Programmable Voltage Supply 2       |
| 59  | PV2        | Programmable Voltage Supply 2       | 60  | PV2        | Programmable Voltage Supply 2       |
| 61  | PV2        | Programmable Voltage Supply 2       | 62  | PV2        | Programmable Voltage Supply 2       |
| 63  | 12VR       | Isolated Relay Supply (+12 V)       | 64  | 12VR       | Isolated Relay Supply (+12 V)       |

> **Note:** Pins marked **(NC)** are Not Connected on the standard interface.

## 3.2 Optional Power Supply Configuration (PS1 – PS6)

For high-current DUT applications, up to **6 optional programmable power supply
modules** (PS1 to PS6) can be configured.

| Signal         | Function                            |
| :------------- | :---------------------------------- |
| **OUT1+, OUT2+** | Positive Outputs                  |
| **OUT1−, OUT2−** | Negative Outputs                  |
| **SEN+**         | Positive Remote Sense             |
| **SEN−**         | Negative Remote Sense             |
| **FG**           | Frame Ground                      |
| **CS**           | Current Sense / Control           |

# 4. System Management Board (SMB) Specifications

The SMB Interface (ver. 12 & above) manages fixture identification, safety
locking, vacuum status, external automation, and onboard auxiliary relay
switching.

## 4.1 SMB Pin Layout (CN10 / CN39)

| Pin No.   | Signal Name       | Description / Electrical Characteristics                        |
| :-------: | :---------------- | :-------------------------------------------------------------- |
| 1, 2      | +12VD             | +12 V Digital Power Line                                        |
| 3 – 10    | STAMP1 – STAMP8   | Open Collector Trigger Lines (Max sink current < 250 mA)        |
| 11, 12    | +24VDA            | +24 V Auxiliary Power Rail A                                    |
| 13 – 20   | ID0 – ID7         | Fixture ID Bit 0 to Bit 7 (Binary address encoding)             |
| 21, 22    | DGND              | Digital Ground Reference                                        |
| 23 – 30   | ID8 – ID15        | Fixture ID Bit 8 to Bit 15 (Binary address encoding)            |
| 31, 32    | DGND              | Digital Ground Reference                                        |
| 33        | FIX_SET           | Fixture Position Lock Sensor (Bit 0)                            |
| 34        | VAC_PIN_CONT      | Vacuum Contact Verification Sensor (Single-sided fixture, Bit 1) |
| 35        | FIX_L_CONT        | Left-side Vacuum Contact Sensor (Dual fixture, Bit 2)           |
| 36        | FIX_R_CONT        | Right-side Vacuum Contact Sensor (Dual fixture, Bit 3)          |
| 37        | FIX_L_PLACE       | Left-side DUT Placement Sensor (Bit 4)                          |
| 38        | FIX_R_PLACE       | Right-side DUT Placement Sensor (Bit 5)                         |
| 39        | INTERFACE         | Interface Engagement Verification Line                          |
| 40        | NC                | Reserved / Unconnected                                          |
| 41 – 48   | EXT_OP1 – EXT_OP8 | External Operator Input Lines (Start, Stop, Safety Interlocks)  |
| 49 – 63   | RLY1 – RLY5       | 5 Sets of onboard Relay Contact Lines                           |
| 64        | +24VDB            | +24 V Auxiliary Power Rail B                                    |
| 65 – 128  | RESERVED          | Do NOT connect (Internal tester routing)                        |

> **Caution:** Pins **65–128** are reserved for internal tester routing and
> must **not** be connected externally.

## 4.2 SMB Onboard Relay Contacts Matrix

The SMB board provides **5 independent form-C software-controlled relays**.

| Relay Channel | Normally Open (RNO) | Common Line (RCOM) | Normally Closed (RNC) |
| :------------ | :------------------ | :----------------- | :-------------------- |
| Relay 1       | Pin 49 (RNO1)       | Pin 50 (RCOM1)     | Pin 51 (RNC1)         |
| Relay 2       | Pin 52 (RNO2)       | Pin 53 (RCOM2)     | Pin 54 (RNC2)         |
| Relay 3       | Pin 55 (RNO3)       | Pin 56 (RCOM3)     | Pin 57 (RNC3)         |
| Relay 4       | Pin 58 (RNO4)       | Pin 59 (RCOM4)     | Pin 60 (RNC4)         |
| Relay 5       | Pin 61 (RNO5)       | Pin 62 (RCOM5)     | Pin 63 (RNC5)         |

> **Form-C note:** Each relay provides a single-pole double-throw (SPDT)
> contact set — the common (RCOM) connects to either RNO or RNC depending on
> the relay's energized state.

# 5. High-Speed Programming (OBP / OBPE) Interface Specifications

On-Board Programming (OBP) and Extended OBP (OBPE) modules execute in-circuit
flash programming and high-speed Boundary Scan (JTAG) testing.

## 5.1 OBP / OBPE Signal Routing & Cable Rules

To prevent clock jitter, crosstalk, and ground bounce during flash programming,
strict cabling standards must be applied.

### Coaxial Cable Requirement

Coaxial wiring must be used for all **critical control bus lines** and **JTAG
clock lines**.

| Category         | Signals                                                |
| :--------------- | :----------------------------------------------------- |
| Control Bus      | R/C# (Pin 35), WR# (Pin 31), CE# (Pin 32), RD# (Pin 30) |
| JTAG Clock Lines | TCLK1 (Pin 87), TCLK2 (Pin 92), TCLK3 (Pin 97)          |

### Twisted-Pair Cable Requirement

Use **AWG 26 or AWG 28** twisted-pair wire for all data/address lines and
remaining JTAG controls.

| Category     | Signals                                                  |
| :----------- | :------------------------------------------------------- |
| JTAG Data    | TDI1–3, TDO1–3, TMS1–3, TRST                             |
| Parallel Bus | Address lines (A0–A25), Data lines (D0–D15)              |

### Ground Shielding Rules

All coaxial outer braids and twisted-pair shield grounds must be tied together
inside the fixture base and bonded to the metal chassis ground.

**Shield Ground Pins (OGND):**

| Pin Numbers                         |
| :---------------------------------- |
| 33, 34, 39, 40, 73, 74, 103, 104    |

# 6. Counter Buffer & Frequency Measurement Specifications

## 6.1 Jumper Configurations (JP2, JP3, JP4)

When expanding counter buffer channel assignments from **8 to 64 measurement
lines**, set the physical jumper switches on the Counter Buffer Card as
follows.

| Active Channel Assignment | JP4  | JP3  | JP2  |
| :------------------------ | :--: | :--: | :--: |
| Channels 1 – 8            |  ON  |  ON  |  ON  |
| Channels 9 – 16           |  ON  |  ON  | OFF  |
| Channels 17 – 24          |  ON  | OFF  |  ON  |
| Channels 25 – 32          |  ON  | OFF  | OFF  |
| Channels 33 – 40          | OFF  |  ON  |  ON  |
| Channels 41 – 48          | OFF  |  ON  | OFF  |
| Channels 49 – 56          | OFF  | OFF  |  ON  |
| Channels 57 – 64          | OFF  | OFF  | OFF  |

> **Note:** The pattern follows a binary encoding — JP4 = MSB, JP2 = LSB.

## 6.2 High-Frequency Signal Line Termination

For all frequency measurement channels routed through the Counter Buffer Card:

- **Standard single wire leads must not be used.**
- Route signal paths via **50 Ω coaxial cabling**.
- Solder a **100 pF ceramic capacitor inline (in series)** with the signal wire
  right at the probe socket terminal.
- Connect the coaxial **outer shield braid directly to the nearest fixture
  ground plane pin within 10 mm** of the probe connection.

# 7. Vectorless & TestJet Wiring Specifications

Vectorless test techniques (e.g., TestJet) detect open IC pins by measuring
capacitive coupling between internal IC lead frames and top-mounted sensor
plates.

## 7.1 TestJet Signal Routing & Noise Isolation

- **Sensor Plate Wiring:**
  Connect top-mounted capacitive sensor plates directly to the TestJet signal
  interface card using **high-impedance shielded coaxial lines**.

- **Noise Mitigation:**
  Keep sensor signal cables separated from high-current power rails
  (**PV1, PV2, 5Va–c**) by a minimum physical clearance of **30 mm** to avoid
  signal coupling.

---

# 8. Switching Boards #1 through #28 Interface Mapping

The TR8000 fixture interface supports up to **28 Switching Boards (SWB)** to
route test pins to the measurement matrix.

## 8.1 Complete SWB Slot & Interface Connector Matrix

| SWB | Interface Connectors | SWB | Interface Connectors |
| --: | :------------------- | --: | :------------------- |
|  1  | CN1, CN2             | 15  | CN17, CN18 (Left) / CN47, CN48 (Right) |
|  2  | CN3, CN4             | 16  | CN19, CN20 (Left) / CN51, CN52 (Right) |
|  3  | CN5, CN6             | 17  | CN21, CN22 (Left) / CN53, CN54 (Right) |
|  4  | CN7, CN8             | 18  | CN23, CN24 (Left) / CN55, CN56 (Right) |
|  5  | CN9, CN10            | 19  | CN25, CN26 (Left) / CN57, CN58 (Right) |
|  6  | CN11, CN12           | 20  | CN27, CN28 (Left) / CN59, CN60 (Right) |
|  7  | CN13, CN14           | 21  | CN29, CN30 (Left) / CN61, CN62 (Right) |
|  8  | CN15, CN16           | 22  | CN49, CN50           |
|  9  | CN41, CN42           | 23  | CN51, CN52           |
| 10  | CN43, CN44           | 24  | CN53, CN54           |
| 11  | CN45, CN46           | 25  | CN55, CN56           |
| 12  | CN47, CN48           | 26  | CN57, CN58           |
| 13  | CN13, CN14           | 27  | CN59, CN60           |
| 14  | CN15, CN16           | 28  | CN61, CN62           |

