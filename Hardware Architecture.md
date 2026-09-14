# 1. System Hardware Architecture

The TR8100LV utilizes a modular backplane architecture to interface measurement modules, power rails, and fixture pin cards.


+-----------------------------------------------------------------------+
|                         SYSTEM CONTROLLER (PC)                        |
+-----------------------------------------------------------------------+
                                   |
                         (System Interface Bus)
                                   v
+-----------------------------------------------------------------------+
|                    SYSTEM MANAGEMENT BOARD (SMB)                      |
|            [System Monitoring / Relays / Fixture Flags]               |
+-----------------------------------------------------------------------+
         |                         |                         |
         v                         v                         v
+-----------------+       +-----------------+       +-------------------+
|  ATM BOARD      |       |  SWB CARDS      |       |  DUT POWER BOARD  |
|  (Analog Meas.) |------>|  (128 Pins/Card)|------>|  (Fixed/Prog DC)  |
+-----------------+       +-----------------+       +-------------------+
         |                         |                         |
         +-------------------------+-------------------------+
                                   v
+-----------------------------------------------------------------------+
|                  BED-OF-NAILS FIXTURE / DUT INTERFACE                 |
+-----------------------------------------------------------------------+

---

# 2. Core Card Specifications

### 2.1 System Management Board (SMB)
The SMB functions as the primary control node for the chassis, handling power sequence execution, safety interlocks, environmental monitoring, and fixture control.

**Main Functions:**
* Monitors internal rail voltages, ambient/chassis temperature, and cooling fan speeds.
* Executes system-level power-on/off sequences.
* Controls fixture state flags (press down/up, lock/unlock status).
* Manages General Purpose I/O (GPIO) lines and user control relays.
* Reads fixture ID codes for automated fixture verification.

## 2.2 Analog Test Module (ATM) Board
The ATM board executes all unpowered analog parametric measurements and guarded component testing.  Main Functions:Performs 2-wire, 3-wire, 4-wire, and 6-wire guarded analog component measurements.  Generates precision AC/DC stimulus voltages and currents.  Evaluates component integrity without applying full power to the PCBA.  Detailed Measurement Ranges:Resistance: 0.1 $\Omega$ to 40 M$\Omega$.  Capacitance: 1 pF to 40 mF.  Inductance: 1 $\mu$H to 60 H.  Zener Diodes: Test range up to 43V.  Active Components: Transistors ($h_{FE}$, $V_{CE}$), MOSFETs/JFETs ($I_{DS}$), SCRs, and TRIACs. 

## 2.3 Manufacturing Defects Analyzer (MDA) Board (TR8001 Systems Only)
The MDA board is dedicated to basic structural, open/short, and high-speed passive component testing.  Main Functions:Rapidly detects manufacturing faults such as solder bridges, open traces, missed components, and reverse polarities.  Provides high-throughput preliminary screening before high-level functional or dynamic testing.  Detailed Specifications:Open/Short Test Threshold: Programmable 1 $\Omega$ to 10 k$\Omega$.Resistance Range: 1 $\Omega$ to 40 M$\Omega$.  Capacitance Range: 10 pF to 40 mF

## 2.4 Switching Board (SWB)
The SWB acts as the matrix multiplexer, routing measurement resources and power rails directly to specific pins on the fixture.  Main Functions:Routes ATM, DUT Power, and signal buses to designated target nodes on the DUT.  Provides true 6-wire guarded measurement paths to eliminate parallel path interference during analog testing.  Detailed Specifications:Pin Density: 128 non-multiplexed physical pins per board.  Guarding Architecture: True 6-wire measurement support per 64-pin block.  Logic Thresholds: Programmable $V_{IH}$ (0V to +4V), $V_{OH}$ / $V_{OL}$ (-5V to +5V).

##  2.5 DUT Power Supply Board (v17 or Later)
Provides clean, software-controlled DC power sources directly to the Device Under Test (DUT) for functional or powered testing.  Main Functions:Supplies logic and analog power rails to the target PCBA.  Prevents overcurrent damage via software-controlled current limiting and hardware shutoff protection.  Output Rails:Fixed Power Channels:+5V @ 5A  +3.3V @ 5A  +12V @ 5A  Programmable Channels:Positive Channel: 0.2V to +20V @ 3A  Negative Channel: -3V to -20V @ 3A


## 2.6 Boundary Scan Board (BScan2)
Executes digital structural testing on IEEE 1149.x compliant integrated circuits without physical bed-of-nails access to every pin.  Main Functions:Tests chip-to-chip interconnects, opens, and shorts using boundary scan chain registers.  Programs on-board Flash, EEPROM, and CPLD memory via standard Test Access Ports (TAPs).  Detailed Specifications:TAPs per Card: 2 independent TAPs per card (expandable up to 32 TAPs total across 16 cards).  Maximum TCK Frequency: Up to 15 MHz.  Supported Standards: IEEE 1149.1, IEEE 1149.6 (AC-coupled differential lines).

## 2.7 On-Board Programming (OBP) Enhancement Board
Provides high-speed parallel and serial in-system flash memory programming directly on the manufacturing line.  Main Functions:Programs MCU flash memory, bootloaders, and non-volatile memory chips post-assembly.  Detailed Specifications:On-Board Buffer Memory: 256 Mb deep memory storage.  Data Bus Width: Up to 16-bit wide parallel bus support.  Clock Speed: Up to 2 MHz test frequency execution.  

## 2.8 TestJet & Counter Module (TCM) Board
Integrates vectorless capacitive IC pin testing with frequency and time interval counter measurements.  Main Functions:Detects open IC pins and cold solder joints without powering up digital components.  Measures clock frequencies, signal periods, and pulse widths on powered PCBAs.  Detailed Specifications:Vectorless Channels: 16 built-in channels (expandable to 64 channels).  Capacitance Resolution: Minimum detectable signal down to 5 fF.  Stimulus Frequency Range: 10 kHz to 100 kHz (300 mVrms to 400 mVrms).  Frequency Counter Range: 10 Hz to 100 MHz.  

## 2.9 Function Interface Board (FIB)
Serves as high-frequency routing hardware between external instruments (oscilloscopes, function generators) and fixture pins.  Main Functions:Maintains signal integrity for high-speed digital or analog signals passing into the fixture.  Detailed Specifications:Coaxial Connectors: 8 SMB coaxial input paths.  Direct Signal Paths: 16 high-bandwidth dedicated lines.  Bandwidth Rating: Up to 160 MHz.  
