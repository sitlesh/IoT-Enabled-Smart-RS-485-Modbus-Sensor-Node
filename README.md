# IoT-Enabled-Smart-RS-485-Modbus-Sensor-Node
Industrial-grade Modbus RTU communication node featuring full galvanic isolation between the ATmega328P logic block and the RS485 network. Designed in KiCad with an optimized on-board LM2596 switching buck regulator. 100% DRC clean.
# Through-Hole Isolated Modbus Board

An industrial-grade, highly reliable Modbus RTU communication hardware platform designed using KiCad. This board acts as a robust node (Master/Slave) in harsh electrical environments, incorporating comprehensive galvanic isolation between the microcontroller logic block and the external RS485 network to safeguard against high-voltage surges, ground loops, and electromagnetic interference (EMI).

---

## 📸 Project Previews
### 3D Render view
<img width="3450" height="1886" alt="IoT-Enabled Smart RS-485 Modbus Sensor Node" src="https://github.com/user-attachments/assets/59b69a74-aa7d-4bda-a0a9-f9ea079babf7" />

---

## 🛠️ Detailed Technical Specifications

### 1. Core Logic & Processing Block
- **Microcontroller:** Microchip ATmega328P-PU (28-pin DIP package for easy through-hole assembly/swapping).
- **Clock Configuration:** External 16 MHz quartz crystal oscillator (`Y1`) stabilized by dual 22pF ceramic load capacitors (`C3`, `C4`) positioned with minimal trace lengths to mitigate high-frequency parasitic layout loop noise.
- **In-System Programming:** Dedicated breakout pins for standard FTDI/UART flashing and debug interfaces.

### 2. Galvanic Isolation Architecture (Dual Plane Separation)
To achieve true industrial-grade isolation, the board is split into two completely distinct galvanic zones divided by a physical isolation trench across the PCB layout:
- **Logic Zone (Safe Side):** Powered by the local regulator system. Houses the ATmega328P, crystal oscillator, and logic-side optocoupler interfaces. Operating on standard `GNDPWR` ground.
- **Bus Zone (Isolated Side):** Completely isolated from the main logic power supply. Houses the MAX485 transceiver, biased pull-up/pull-down safety networks, and external communication lines. Operating on isolated `GNDD` ground.
- **Signal Opto-coupling:** Dual high-speed optocouplers (`PC817`) handle bidirectional UART data flow ($TX$ and $RX$). 
  - `U4` isolates transmission signals going from the MCU to the transceiver.
  - `U3` isolates receiving network data going from the transceiver back to the MCU.

### 3. Power Supply & Regulation Block
- **Topology:** Step-down switching buck regulator circuit utilizing the **LM2596T-5** IC to step down standard industrial 12V/24V DC inputs down to a stable 5V rail.
- **Input Filtering & Protection:** Reverse polarity protection diode (`D1` - 1N4007) paired with a high-capacity polarized input electrolytic smoothing capacitor (`Cin1`).
- **Power Loop Optimization:** Low-loop-inductance component placement topology linking the input capacitor, switching regulator output pin, fast-recovery Schottky catch diode (`D2` - 1N5822), power inductor (`L1`), and output filtering capacitor (`Cout1`) linearly to dramatically minimize switching EMI.

### 4. RS485 Modbus Interface
- **Transceiver:** Maxim Integrated **MAX485E** low-power transceiver for RS485 communication.
- **Bus Termination:** On-board standard $120\Omega$ termination resistor (`R5`) bridging the differential $A$ and $B$ communication lines to eliminate signal reflections over long cable distances.
- **Connectors:** Heavy-duty screw terminals (`J1` for main DC power input, `J2` for differential $A/B$ Modbus lines) accommodating industrial wiring configurations.

---

## 📐 PCB Layout Design Parameters

- **EDA Tool:** KiCad 10.0
- **Form Factor Dimensions:** $80\text{mm} \times 50\text{mm}$
- **Layer Count:** 2 Layers (Top Layer: Signals & Linear Power Paths / Bottom Layer: Split Solid Ground Planes for optimal shielding).
- **Track Routing Guidelines:** - Signal Traces: `0.30mm` to `0.40mm` width for tight digital density.
  - High-Current Power Traces: `0.80mm` to `1.00mm` width to ensure minimal voltage drops and lower thermal resistance.
  - Geometry: Strict usage of $45^\circ$ angles to eliminate current crowding and signal distortion.
- **Design Rule Verification:** 100% DRC (Design Rule Check) compliant with **0 Errors** and **0 Warnings**.

---

## 📂 Repository Directory Structure

```text
├── Hardware/          # Core EDA Design Files
│   ├── *.kicad_sch    # Schematic Wiring Diagrams
│   ├── *.kicad_pcb    # 2-Layer PCB Component Layout Rules
│   └── *.kicad_pro    # KiCad Active Workspace Configuration
├── Manufacturing/     # Production Release Packages
│   └── Gerbers.zip    # Standard RS-274X Gerber & Drill Fabrication files
├── Docs/              # Technical Records & Visual Attachments
│   ├── BOM.csv        # Detailed Bill of Materials with part designations
│   └── *.png          # High-resolution 3D Top/Bottom layout renders
└── README.md          # Primary Documentation Core
