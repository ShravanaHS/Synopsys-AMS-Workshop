# Synopsys-AMS-Workshop

# CMOS Inverter Design using Synopsys Tools

In this repository, I am designing, simulating, and verifying a **CMOS Inverter** using the **[Your PDK, e.g., SkyWater 130nm]** Process Design Kit.

The entire design and verification flow is carried out using the Synopsys tool suite:
* **Schematic and Layout:** Synopsys Custom Compiler™
* **Pre-Layout & Post-Layout Simulation:** Synopsys PrimeSim™ HSPICE™
* **Physical Verification (DRC & LVS):** Synopsys IC Validator
* **Parasitic Extraction:** Synopsys StarRC™

---

## Abstract

This repository documents the design and analysis of a fundamental CMOS Inverter. The inverter is the most basic logic gate in digital electronics, serving as the foundation for more complex digital circuits. Its primary function is to invert the input logic level. This project covers the complete design flow, from schematic creation and pre-layout simulation to physical layout, verification (DRC/LVS), parasitic extraction, and final post-layout simulation to analyze the impact of physical effects on circuit performance.

---

## Functionality

The CMOS Inverter operates on a simple principle:
1.  **When the input voltage is HIGH (logic 1):** The NMOS transistor turns ON, and the PMOS transistor turns OFF. This creates a direct path from the output to Ground (GND), pulling the output voltage to LOW (logic 0).
2.  **When the input voltage is LOW (logic 0):** The PMOS transistor turns ON, and the NMOS transistor turns OFF. This creates a path from the output to the supply voltage (VDD), pulling the output voltage to HIGH (logic 1).

A key advantage of the CMOS inverter is its extremely low static power consumption, as one of the transistors is always OFF in a steady state.

---

## Truth Table

| Input (Vin) | Output (Vout) |
|:-----------:|:-------------:|
|      0      |       1       |
|      1      |       0       |

---

## Symbol of Inverter

<div align="center">
    <img src="images/inverter_symbol.png" alt="Inverter Symbol">
    <p><strong>Logic Symbol of an Inverter</strong></p>
</div>

---

## Circuit Diagram (Schematic)

The CMOS inverter consists of one PMOS transistor and one NMOS transistor connected in series between the power supply (VDD) and Ground (GND).
- The gates of both transistors are tied together to form the input (Vin).
- The drains of both transistors are tied together to form the output (Vout).

This schematic was created using **Synopsys Custom Compiler™**.

<div align="center">
    <img src="images/inverter_schematic.png" alt="CMOS Inverter Schematic">
    <p><strong>CMOS Inverter Circuit Schematic</strong></p>
</div>

---

## Pre-Layout Simulation Analysis

Pre-layout simulations are performed on the schematic using **Synopsys PrimeSim™ HSPICE™** to verify the intended functionality before proceeding to the physical layout design.

### DC Analysis and VTC

A DC sweep analysis is performed by sweeping the input voltage (Vin) from 0V to VDD to obtain the Voltage Transfer Characteristic (VTC) curve. The VTC is crucial for determining key static parameters of the inverter.

-   **$V_{OH}$ (Output High Voltage):** Maximum output voltage, ideally VDD.
-   **$V_{OL}$ (Output Low Voltage):** Minimum output voltage, ideally 0V.
-   **$V_{IH}$ (Input High Voltage):** The input voltage at which the slope of the VTC is -1.
-   **$V_{IL}$ (Input Low Voltage):** The input voltage at which the slope of the VTC is -1.
-   **$V_{M}$ (Switching Threshold Voltage):** The input voltage where $V_{in} = V_{out}$.
-   **Noise Margins:** $NM_H = V_{OH} - V_{IH}$ and $NM_L = V_{IL} - V_{OL}$.

<div align="center">
    <img src="images/inverter_vtc.png" alt="Inverter VTC Curve">
    <p><strong>Voltage Transfer Characteristic (VTC) of the Inverter</strong></p>
</div>

### Transient (Delay) Analysis

Transient analysis is used to measure the dynamic performance of the inverter. A pulse input is applied, and the output waveform is observed to calculate propagation delays and transition times.

-   **$t_{pHL}$ (Propagation Delay High-to-Low):** Time taken for the output to fall to 50% of its final value after the input crosses its 50% point.
-   **$t_{pLH}$ (Propagation Delay Low-to-High):** Time taken for the output to rise to 50% of its final value after the input crosses its 50% point.
-   **$t_r$ (Rise Time):** Time for the output to rise from 10% to 90% of its final value.
-   **$t_f$ (Fall Time):** Time for the output to fall from 90% to 10% of its initial value.

<div align="center">
    <img src="images/inverter_delay.png" alt="Inverter Delay Waveform">
    <p><strong>Transient Response for Delay Calculation</strong></p>
</div>

---

## Physical Design (Layout)

The physical layout of the inverter was created in **Synopsys Custom Compiler™**. This involves drawing the physical layers (like N-well, active areas, polysilicon gates, and metal interconnects) according to the design rules of the technology.

<div align="center">
    <img src="images/inverter_layout.png" alt="CMOS Inverter Layout">
    <p><strong>Layout of the CMOS Inverter</strong></p>
</div>

---

## Physical Verification and Extraction

### DRC and LVS

After completing the layout, physical verification is performed using **Synopsys IC Validator**.
-   **Design Rule Check (DRC):** Ensures the layout does not violate any manufacturing rules specified by the foundry.
-   **Layout Versus Schematic (LVS):** Compares the netlist extracted from the layout with the original schematic netlist to ensure they are electrically identical.
The layout passed both DRC and LVS checks successfully.

### Parasitic Extraction (LPE)

After LVS is clean, **Synopsys StarRC™** is used to perform Layout Parasitic Extraction (LPE). This tool extracts the parasitic resistances (R) and capacitances (C) from the physical layout, which are caused by the interconnects and device structures. This results in a more realistic netlist for post-layout simulation.

---

## Post-Layout Simulation

Post-layout simulations are performed using **Synopsys PrimeSim™ HSPICE™** with the parasitic-annotated netlist from StarRC. This simulation provides a more accurate prediction of the circuit's performance, as it includes the delays and signal integrity issues caused by the physical layout. The same transient analysis is run to compare the pre-layout and post-layout delay values.

| Parameter | Pre-Layout Value | Post-Layout Value | Unit |
|:--- |:---:|:---:|:---:|
| **$t_{pHL}$** | `XX` | `YY` | ps |
| **$t_{pLH}$** | `XX` | `YY` | ps |
| **Rise Time** | `XX` | `YY` | ps |
| **Fall Time** | `XX` | `YY` | ps |

---

## Conclusion

This project successfully demonstrated the complete design and verification flow of a CMOS inverter using the Synopsys suite of EDA tools. The analysis confirmed the inverter's core functionality and characterized its static (VTC) and dynamic (delay) performance. The comparison between pre-layout and post-layout simulations highlighted the critical impact of layout parasitics on circuit speed, reinforcing the importance of a full physical design and verification cycle.
