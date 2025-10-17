### Core Components
- **Jetson AGX Orin (32 GB)** or **64GB variant** — main compute (AI, HMI, fusion)
- **TI FPD-Link IV chain**
  - DS90UB960: 4× RX deserializer to MIPI CSI-2
  - DS90UB953: serializer per camera with STP
- **STM32 MCU**
  - Watchdog, CAN gateway, alert control, power logic
- **ESP32**
  - Secondary I/O, telemetry, wireless interface
- **DC/DC Converter**
  - 12 V → 12 V @ ≥150 W, EMI filtered, thermally managed
- **Supercap or UPS**
  - Clean shutdown during crank voltage drops
- **Touch Display (13.3" ATNA33XC21-0)**
  - HDMI/DP video, USB touch input, 10-bit color, 550 nits brightness

### Wiring Notes
- Shielded signal lines (especially coax and CAN).
- Star grounding layout to avoid ground loops.
- Route PoC lines away from ignition and motor control wiring.
