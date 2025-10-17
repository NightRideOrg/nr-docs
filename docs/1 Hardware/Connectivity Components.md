### Overview

This document outlines the selection of core electronic components for the "NightRide" automotive connectivity hub. The system is divided into three main subsystems for clarity and modularity:
1.  **Primary Connectivity:** High-speed 5G data and in-vehicle Wi-Fi hotspot.
2.  **Positioning & Dynamics:** High-precision navigation and vehicle motion tracking.
3.  **Security & Redundancy:** A low-power, independent tracking and fallback communication system.

---

## I. Primary Connectivity System

This system provides the main high-bandwidth data link for the vehicle and its occupants.

### 5G Cellular Modem

> [!note] Module: **Fibocom FM160-EAU**
> A high-performance 5G NR Sub-6 module based on the Qualcomm X62 (Rel-16) chipset, selected for its modern feature set and robust performance.

| Specification | Details |
| :--- | :--- |
| **Air Interface** | 5G NR Sub-6 (SA/NSA), LTE-A fallback |
| **EU Bands** | **5G:** n1, n3, n5, n7, n8, n20, n28, n38, n40, n41, n75, n76, n77, n78<br>**LTE:** B1/3/5/7/8/20/28/32 + B38/40/41/42/43 |
| **Peak Rate (NSA)** | **~3.5 Gbps DL** / 0.9 Gbps UL |
| **MIMO** | **4×4 DL** on key 5G/LTE bands |
| **Interface** | USB 3.1 (preferred), PCIe 4.0 |
| **Power Supply** | 3.135V – 4.4V (3.8V typ.) |
| **Temp Range** | -30°C to +75°C |

> [!warning] Critical Design Constraint
> The power supply circuit must be designed to handle **transient current peaks exceeding 4A** during TX bursts to prevent voltage droop and modem resets.

> [!success] Key Automotive Advantages
> - **Future-Proof Platform:** The Release-16 chipset supports NR Carrier Aggregation (NR-CA) for higher throughput and more robust connections while mobile.
> - **Excellent EU Coverage:** Covers all critical 5G and LTE bands for pan-European operation. The absence of n79 is irrelevant for Germany/most of EU.
> - **High Throughput:** 4x4 MIMO support is essential for achieving multi-gigabit speeds and requires a 4-antenna configuration.

---

### Wi-Fi & Bluetooth Hotspot

> [!note] Module: **Intel BE200 (Wi-Fi 7)**
> A latest-generation M.2 module providing tri-band Wi-Fi 7 and Bluetooth 5.4, creating a powerful and future-proof in-vehicle hotspot.

| Specification | Details |
| :--- | :--- |
| **Air Interface** | Wi-Fi 7 (802.11be), Bluetooth 5.4 |
| **Bands** | 2.4 GHz, 5 GHz, **6 GHz** (Tri-Band) |
| **MIMO** | **2x2 MIMO** |
| **Interface** | M.2 A+E Key (PCIe for Wi-Fi, USB for BT) |
| **Temp Range** | 0°C to +50°C (Ambient) |

> [!abstract] Recommended Antenna: **Taoglas GW55.A.07.B.001**
> A compact 2-in-1 MIMO antenna unit in a single housing. It connects via two U.FL cables to both antenna ports on the BE200.

> [!success] Key Automotive Advantages
> - **Simplified MIMO Installation:** A single antenna unit provides the two feeds required for 2x2 MIMO, drastically simplifying cable routing and placement under the dashboard.
> - **Integrated Functionality:** The BE200 provides both a high-speed Wi-Fi 7 hotspot and Bluetooth 5.4 for peripherals (e.g., Phones) using the same shared antennas, reducing complexity.
> - **Optimized for In-Vehicle Use:** The GW55 antenna is designed to create a strong, local Wi-Fi bubble inside the cabin, providing excellent coverage for all occupants.

---

## II. Positioning & Dynamics System

This system provides the primary navigation data and detailed insights into vehicle movement.

### Primary GNSS (Navigation)

> [!note] Module: **u-blox NEO-M9V**
> A standard precision GNSS module with an integrated IMU for Multi-mode Dead Reckoning (MDR), ensuring continuous positioning.

| Specification | Details |
| :--- | :--- |
| **Technology** | Concurrent GNSS (Galileo, GPS, GLONASS, BeiDou) + Dead Reckoning |
| **Accuracy** | Meter-level. Tunnel Error: **~2%** (with wheel-tick), ~10% (IMU only) |
| **Key Features** | Integrated 6-axis IMU, Anti-Jamming/Spoofing, 50 Hz update rate |
| **Interfaces** | UART, I²C, USB, **Wheel Tick & Direction Inputs** |
| **Temp Range** | -40°C to +85°C (Professional Grade) |

> [!success] Key Automotive Advantages
> - **100% Positioning Coverage:** Dead Reckoning provides a continuous, stable position fix through tunnels, parking garages, and urban canyons where satellite-only modules fail.
> - **Superior Accuracy (ADR):** The option to connect to the car's wheel speed sensor (Automotive Dead Reckoning mode) offers exceptional DR performance.
> - **Fast & Reliable Fix:** Concurrent reception of 4 satellite constellations, including Europe's Galileo, ensures a rapid and robust lock.

---

### High-Fidelity Vehicle Dynamics

To accurately log vehicle dynamics and provide absolute heading, a combination of a high-grade IMU and a thermally stable magnetometer is required.

> [!note] Core Motion Sensor (IMU): **STMicroelectronics ASM330LHB**
> An automotive-grade, high-performance 6-axis IMU (Accelerometer + Gyroscope) purpose-built for harsh vehicle environments. It provides the raw acceleration and angular rate data.
>
> | Specification | Details |
> | :--- | :--- |
> | **Qualification** | **AEC-Q100 Automotive Grade**, ASIL B Compliant |
> | **Gyro Noise** | **5 mdps/√Hz** (Ultra-low) |
> | **Temp Range** | **-40°C to +105°C** |
> | **Features** | On-chip Machine Learning Core (MLC) |

> [!abstract] Heading Reference (Magnetometer): **STMicroelectronics IIS2MDC**
> A high-accuracy, industrial-grade 3-axis magnetometer. It acts as the absolute heading reference, complementing the relative rotational data from the IMU's gyroscope.
>
> | Specification | Details |
> | :--- | :--- |
> | **Dynamic Range** | **±50 gauss** (Resists magnetic interference) |
> | **Temp Stability** | **±0.03 mgauss/°C** (Excellent) |
> | **Temp Range** | -40°C to +85°C |

> [!success] Why this combination is ideal:
> - **Purpose-Built for Automotive:** The AEC-Q100 qualification of the IMU guarantees reliability. The magnetometer's industrial grading and exceptional thermal stability ensure heading accuracy is consistent from a cold start to a hot day.
> - **Superior Data Quality:** The extremely low noise of the IMU is critical for rejecting road/engine vibrations, while the magnetometer's high dynamic range prevents sensor saturation from the car's own electrical systems. Together, they provide clean, reliable data for sensor fusion.

---

## III. Security & Redundancy System

This subsystem is designed for ultra-low power, intermittent operation to serve as an independent security tracker and communication fallback. It can wake on movement or operate on a timer (e.g., every 15 minutes) to report its status while minimizing battery drain.

### Security GNSS (Tracker)

> [!note] Module: **u-blox MAX-M10S**
> An ultra-low-power standard precision GNSS receiver optimized for battery-powered asset tracking applications.

| Specification | Details |
| :--- | :--- |
| **Technology** | Concurrent GNSS (Galileo, GPS, etc.) |
| **Power Draw** | **~15 mA** (Continuous Tracking) |
| **Sensitivity** | **-167 dBm** (Tracking) |
| **Key Features** | Anti-Jamming & Spoofing Detection, Advanced Power Save Modes |

> [!abstract] Recommended Antenna: **Taoglas AGGBP.25B**
> An automotive-grade active patch antenna with an integrated SAW filter, providing the perfect 28dB gain for the MAX-M10S and excellent noise rejection. Its cabled form factor allows for optimal hidden placement.

> [!success] Key Automotive Advantages
> - **Minimal Power Drain:** Ultra-low consumption is ideal for an intermittently-powered device connected to the car battery, ensuring negligible parasitic drain when asleep.
> - **Acquires Lock in Hostile Conditions:** Industry-leading sensitivity is critical for a hidden tracker, allowing it to get a fix from deep within the car's structure or in a garage.
> - **Turns Signal Loss into an Alert:** Intelligent jamming detection provides a high-confidence theft alert, unlike a simple, ambiguous signal loss.

---

### LPWA (SMS/Data Fallback)

> [!note] Module: **Fibocom MA510-GL**
> A global LTE Cat-M1/NB2 module with 2G fallback for ultimate network availability in a compact, low-power package.

| Specification | Details |
| :--- | :--- |
| **Air Interface** | LTE Cat-M1, Cat-NB2, EGPRS (2G) |
| **EU Bands** | B3, B8, B20, B28 (LTE-M/NB) + 900/1800MHz (2G) |
| **Interface** | UART (x3), USB 2.0, I2C, GPIO |
| **Power** | Low power with PSM and eDRX modes |
| **Temp Range** | -40°C to +85°C |

> [!abstract] Recommended Antenna: **Taoglas MFX3.07.0150C**
> A flexible FPC antenna specifically tuned for LPWA bands. Its larger size and high efficiency provide predictable, superior performance compared to tiny SMD antennas, ensuring a message can get out even from a difficult location.

> [!success] Key Automotive Advantages
> - **Maximum Network Redundancy:** Provides a low-cost, low-power communication channel completely independent of the 5G modem. The combination of modern LPWA and legacy 2G ensures a connection is almost always possible.
> - **Perfect for MCU Control:** Designed for simple AT command control over UART, making it ideal for a host microcontroller (e.g., ESP32) to manage out-of-band alerts.
> - **Deep In-Vehicle Coverage:** LTE-M and 2G bands (like B20/800MHz and 900MHz) are excellent at penetrating structures, increasing the chance of a successful transmission from inside a building or garage.

---

## IV. Antenna Strategy Summary

| System              | Feeds | Recommended Antenna Type | Notes                                                          |     |
| :------------------ | :---: | :----------------------- | :------------------------------------------------------------- | --- |
| **5G / LTE (Main)** |   4   | 4x4 MIMO Sub-6 GHz       | Primary data link. Requires high-performance, spaced antennas. |     |
| **Primary GNSS**    |   1   | Active L1/L5 GNSS        | For main navigation system. Must have clear sky view.          |     |
| **Security GNSS**   |   1   | Active L1 GNSS           | For hidden tracker. Placed under non-metallic trim.            |     |
| **LPWA Fallback**   |   1   | LTE Cat M/NB/2G          | For hidden tracker. Can be a flexible internal antenna.        |     |
| **Wi-Fi / BT**      |  (2)  | 2x2 MIMO Wi-Fi           | *Handled by the single internal GW55 antenna unit.*            |     |

> [!tip] Recommended Primary Roof Antenna: **Panorama GPSD4-6-60**
> This is an excellent "all-in-one" roof-puck solution that covers all the needs for the primary systems, greatly simplifying the vehicle installation.
> - **4x4 MIMO** for 4G/5G (617-6000 MHz)
> - **1x Active GNSS** feed (GPS/GLONASS/Galileo/BeiDou)
>
> This single unit handles 5 of the 7 required feeds with a professional, IP69K-rated housing, requiring only a single mounting hole in the vehicle roof. The remaining security/LPWA antennas would be mounted covertly inside the vehicle.