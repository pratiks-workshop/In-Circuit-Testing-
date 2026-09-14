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

# 2. Core Card Specifications

