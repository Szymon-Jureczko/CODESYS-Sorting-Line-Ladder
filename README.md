# Automated 3-Bin Conveyor Sorting System

A PLC control program for an automated conveyor sorting line, developed in CODESYS. The main machine logic is written entirely in IEC 61131-3 Ladder Diagram (LD), demonstrating standard industrial motor control, sensor-based routing, and robust safety interlocking.

This project simulates a high-speed sorting belt where incoming items are identified by photo-eye sensors and routed into appropriate bins using pneumatic pushers. 

## Key Engineering Features

* **Standard Motor Control:** Implements Start/Stop latching circuits (seal-in logic) for the system master power and main conveyor drive, complete with Normally Closed (NC) safety interlocks.
* **Hardware Safety Interlocks:** Actuator networks are strictly interlocked with the `xSystemRunning` state. This ensures that physical pushers cannot fire if a sensor is accidentally tripped while the machine is powered down.
* **Sensor-Driven Routing:** Utilizes comparator blocks (`EQ`) tied to sensor inputs to conditionally route power to the correct actuator networks based on item type IDs (1, 2, or 3).
* **Timed Actuator Control:** Integrates Pulse Timers (`TP`) to convert fleeting 1-second sensor detection blips into sustained 1.5-second signals. This ensures the pneumatic pushers fully extend and retract, preventing physical jams and allowing for clear visual verification.
* **Internal Simulation Engine:** Features a decoupled "Virtual Factory" script written in Structured Text (ST). This background task acts as a heartbeat generator, spawning items every 4 seconds and triggering virtual sensors to rigorously test the Ladder Logic without requiring physical hardware. The simulation is also conditionally interlocked to halt when the main system stops.

## Tech Stack & Tools

* **Environment:** CODESYS V3.5
* **Primary Control Logic:** IEC 61131-3 Ladder Diagram (LD)
* **Simulation Engine:** IEC 61131-3 Structured Text (ST)
* **Core Concepts:** Latching Circuits, Pulse Timers, Comparator Logic, Boolean Algebra, Safety Interlocking, Hardware Simulation.

## Logic Overview

*(Note: Replace this text with a screenshot of your clean Ladder Logic—specifically the network showing the Start/Stop latch, and one of your pusher networks with the TP timer. Employers love seeing well-structured, readable ladder code!)*
[Insert Screenshot of Ladder Logic here]

## How to Run the Simulation

1. Clone this repository and open the `.project` file in CODESYS.
2. Ensure your local gateway is running (`CODESYS Control Win V3 x64`).
3. Double click the Device in the tree and hit **Scan Network** to connect.
4. Go to **Online -> Login**, then press **Start (F5)**.
5. In the `PLC_PRG` (Ladder) window, manually toggle `xSystemStart` to TRUE (Ctrl+F7) and then back to FALSE to latch the system ON.
6. The ST simulation program will automatically begin generating items and triggering the pushers in the Ladder logic every 4 seconds. Toggle `xSystemStop` to verify the safety interlocks gracefully halt the simulation and lock out the actuators.

## Author
Szymon Jureczko
