# TTL Logic Test Theory

## Gate types
Bi-state gates (AND, OR, XOR, NOT, NAND, NOR, flip-flops) plus open-drain and tri-state devices. Tested via truth-table vector patterns.

## Pin classification
Output, bi-state output, tri-state output, open-drain, bidirectional, power.

## Critical levels
- VIH must exceed the datasheet VIH(min) and stay below VCC.
- VIL must be below datasheet VIL(max).

## Test sequence
1. Apply all input-pin data.
2. Latch trigger-type inputs last (e.g. 74380).
3. Sense output pins in pin order.

## Group setup 
GROUP = 1,3
2,1,4
The pin order in `#GROUP` controls which input is driven first, allowing path-specific conditions.

## Bus sharing
If U1, U2, U3 share a bus, float U2/U3 outputs before testing U1.

# Tree-Chain Test

## Concept
Internal chain of gates (AND/NAND historically; XOR now) connecting I/O pins. Test reveals open pins quickly with few vectors.

## Preconditions
- IC must support Tree Chain.
- Test mode must be entered with the vendor-specific command.

## Algorithm
For each input pin i:
1. (Non-AND trees) drive PIN[i−1] and PIN[i−3] low.
2. Drive PIN[i] low → sense output (V1).
3. Drive PIN[i] high → sense output (V2).
4. If V1 == V2, PIN[i] fails.

The output is read only at the transition — a fixed output means the chain is broken at that point.

# Memory Test Theory

## Cell test scope
- **Full cell** — every address; slow, used at IC fabrication.
- **Partial cell** — a designed subset of addresses/data; sufficient for production ICT.

## Walking One (address bus)
For each address line Ai:
1. Write 0x01 to address with Ai = 1.
2. Write 0x00 to address 0.
3. Read the Ai=1 address — expect 0x01.

If Ai is stuck low, the read returns 0x00 (written to address 0) instead of 0x01.

## Walking Zero (address bus)
Same pattern with the bit inverted — detects stuck-high lines.

## Data bus
Write 0x00 / 0xFF and read back; per-bit patterns (e.g. 0b01000000) isolate stuck bits:
- Stuck-high D6: writing 0b00000000 reads 0b01000000.
- Stuck-low D2: writing 0b11111111 reads 0b11111011.

# I2C Basics

 Inter-Integrated Circuit — two-wire bus (SDA, SCL) for on-board control.

## Uses in ICT
- Program clock generators (change output frequency, disable outputs).
- Set up DUT state before functional measurement.

## Protocol
1. Master sends START.
2. Address byte (7-bit address + R/W).
3. Slave ACKs.
4. Data bytes, each ACKed.
5. Master sends STOP.

## Signal integrity
SCL is the data synchronizer; every bit must be stable while SCL is high. START/STOP are SDA transitions while SCL is high.

## Modes
- Byte Read / Byte Write
- Sequential Read / Sequential Write (multiple DATA n)

# P2C Basics

Texas Instruments PCMCIA Peripheral Control — 3-wire interface (DATA, CLOCK, LATCH).

## Uses in ICT
Example: PCMCIA card power switch — write output-voltage control commands, then measure the output.

## Signals
- DATA — serial data
- CLOCK — data sync
- LATCH — data latch

Functionally similar to I2C but with an explicit LATCH signal.

# Boundary-Scan (JTAG / IEEE 1149.1)

## Why
Bed-of-nails access shrinks as BGA/QFN pin counts rise. Boundary scan embeds a shift register cell at every digital I/O so pins can be controlled and observed serially.

## Requirements
- DUT supports BSCAN.
- BSDL file available.

## TAP pins
| Pin | Meaning |
|-----|---------|
| TDI | Test Data In |
| TDO | Test Data Out |
| TCK | Test Clock |
| TMS | Test Mode Select |
| TRST | Test Reset (optional) |

## Architecture
- **TAP Controller** — FSM driven by TCK/TMS/TRST.
- **Instruction Register** — selects EXTEST, SAMPLE, BYPASS, IDCODE, etc.
- **Data Registers** — Bypass (1 cell), Device ID (32-bit), Boundary Register (1–3 cells per I/O pin).
- **Cell types** — Input, Output, Bidirectional, Control.

## Chain test
TDI → cell 0 → cell 1 → … → TDO. Shift a pattern, capture at TDO; a mismatch identifies the bad interconnect or open pin.

## Example
Two BSCAN ICs (U1, U2) around an SDRAM — U1 drives a pin, U2 samples it (or the interconnect is tested via EXTEST).

# Flash Memory Test

## Command set
Check ID, Erase, Verify, Read, Compare, Program, Read Status.

## Typical sequence (programming path)
1. Check ID (confirm vendor).
2. Blank check.
3. Erase.
4. Program.
5. Verify.

If data is pre-burned, verification alone suffices.

## Notes
- Read algorithm matches standard ROM.
- Write/erase algorithms are vendor-specific.
- Initialization sequences write specific data to specific addresses.
- Macro commands assemble vendor-specific sequences.
- Watch total test time — full program cycles are slow.
- Ensure upstream chips are controlled/floating before driving the flash bus.

# MWIRE Serial EEPROM

## Pins
- CS — Chip Select
- CLK — Serial Clock
- DI — Data In
- DO — Data Out
- ORG — 8-bit vs 16-bit organization

## Part numbers
Start with 93, e.g. **93C46**, **93C66**.

## Use case
Commonly stores the MAC address in network products.

## Testing
Read/Write commands are issued per the MWIRE protocol; verify read-back for program success.

# ISP Test Theory

In-System Programming for CPLD/FPGA devices (Altera, Xilinx).

## Flow
1. Read the vendor programming file.
2. Erase.
3. Program.
4. Verify.

## Via JTAG
Uses the standard JTAG pins (TCK, TMS, TDI, TDO). Primary risk is a broken serial chain — verify JTAG connectivity before attempting programming.

# SPI Serial Memory

## Pins
CS, CLK, DI, DO — same four-wire topology as MWIRE but different protocol and command set.

## Categories
- Serial EEPROM (25-series, e.g. 25C20)
- Serial Flash (e.g. Atmel AT25F1024N)

## Use case
BIOS storage — serial flash offers larger capacity than EEPROM.

## Testing
Read ID (RDID), read status, read data; verify against expected image.

# Consolidated Mode Reference

## Resistor
| Mode | Description |
|------|-------------|
| 0 | Constant current |
| 1 | Low constant current |
| 2 | High-speed 0.2 V |
| 3–5 | AC phase (1 k / 10 k / 100 k) |
| 6 | 4-wire |
| 7 | 4-wire fast |
| 8 | 6-wire with guard |

## Capacitor
| Mode | Description |
|------|-------------|
| 0–3 | AC 1 k / 10 k / 100 k / 1 M |
| 4 | DC 5 mA |
| 5–7 | AC phase |
| 8 | DC 10 mA |
| 11 | AC 1 k SMD (0.35 V) |
| 12 | AC 1 k phase wide range |
| 14 | AC 100 k phase wide range |
| 15 | AC 1 M 4-wire |

## Inductor
| Mode | Frequency |
|------|-----------|
| 0–3 | 1 k / 10 k / 100 k / 1 M |
| 5–7 | Phase variants |
| 9 | 100 Hz |

## Transistor
| Mode | Function |
|------|----------|
| 3 | PNP VCE |
| 4 | NPN VCE |
| 12/13 | Hfe (200 Ω) |
| 32/33 | Hfe (20 Ω) |
| 34–37 | ΔHfe (2-phase) |

## FET
| Mode | Device |
|------|--------|
| 14 | N-MOSFET |
| 15 | P-MOSFET |
| 18 | N-JFET |
| 19 | P-JFET |
| 20 | N-JFET Vds |
| 21 | P-JFET Vds |
| 26 | P-type 2-source |
| 27 | N-type 2-source |
| 28/30 | N-FET Rds (20/200 Ω) |
| 29/31 | P-FET Rds (20/200 Ω) |

## Diode
| Mode | Function |
|------|----------|
| 0/1/24/25 | Forward knee |
| 5/6 | Type D / Type HV |

# Glossary

| Term | Definition |
|------|------------|
| **Raw THD** | Initial threshold (Short + 200 Ω) used during Raw Test to flag suspicious shorts |
| **Open THD** | Maximum impedance allowed inside a learned short group |
| **Short THD** | Threshold below which a connection is a true short |
| **OPI** | Open-Impedance test — every pair in a short group measured for opens |
| **SPI** | Short-Impedance test — non-shorted pairs measured for shorts |
| **Guard** | Driving a node to the same potential as the measured node to block shunt current |
| **MOA_R** | Feedback resistor in the AC measurement op-amp; smaller = wider current range |
| **Rds** | Drain-source resistance (FET) |
| **Hfe** | Transistor current gain (Ic/Ib) |
| **TAP** | Test Access Port (JTAG) |
| **BSDL** | Boundary-Scan Description Language file |
| **ISP** | In-System Programming |
| **MWIRE** | Microwire serial EEPROM protocol (93-series) |
| **SPI** | Serial Peripheral Interface (25-series) |
| **Phantom short** | False short caused by parallel resistor paths |
| **Suspicious short** | Nail flagged during Raw Test that requires Fine Test to confirm |
