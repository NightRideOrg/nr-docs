# Rosenberger HSD Connector System for Automotive Camera Project

This document specifies the Rosenberger HSD connectors and coding scheme for the 7-camera system. The system uses a 100Ω Shielded Twisted Pair (STP) cable for the FPD-Link III interface.

**System Pinout Standard (on HSD Connector):**
*   **Pin 1:** Data+ (Differential Pair)
*   **Pin 2:** Data- (Differential Pair)
*   **Pin 3:** Ground
*   **Pin 4:** Vehicle Power (Vbat, e.g., +12V)

---

## Connector Bill of Materials (BOM)

| Camera Role | Coding Letter | Color (RAL-similar) | Plug (On Camera PCB) | Jack (On ECU PCB) |
| :--- | :--- | :--- | :--- | :--- |
| **Front Grill** | A | Jet Black (9005) | `D4S20Y-40MA5-A` | `D4K20E-1D5A5-A` |
| **Mirror Left** | B | Cream White (9001) | `D4S20Y-40MA5-B` | `D4K20E-1D5A5-B` |
| **Mirror Right** | C | Signal Blue (5005) | `D4S20Y-40MA5-C` | `D4K20E-1D5A5-C` |
| **Rear** | D | Claret Violet (4004) | `D4S20Y-40MA5-D` | `D4K20E-1D5A5-D` |
| **Windshield Wide** | E | Leaf Green (6002) | `D4S20Y-40MA5-E` | `D4K20E-1D5A5-E` |
| **Windshield Tele** | F | Nut Brown (8011) | `D4S20Y-40MA5-F` | `D4K20E-1D5A5-F` |
| **Driver IR Cam** | G | Blue Grey (7031) | `D4S20Y-40MA5-G` | `D4K20E-1D5A5-G` |

---
**Notes:**
*   `D4S20Y-40MA5-y`: Right Angle HSD Male Plug, Pin-in-Paste, for Camera PCB.
*   `D4K20E-1D5A5-y`: Right Angle HSD Female Jack, Cable-down, for ECU PCB.
*   The `-y` placeholder in the part number is replaced by the specific **Coding Letter**.
*   Cable assemblies must be ordered with matching male plugs and codings on both ends.