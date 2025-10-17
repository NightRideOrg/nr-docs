# Project: DIY Lane Change Assist for Volvo V60 (2015)

**Owner:** Niclas  
**Core HW:** Jetson AGX Orin (32GB), TI FPD-Link IV (DS90UB960/953), STM32 Safety MCU, ESP32 Telemetry, 13.3" 1920×1080 10-bit 550-nit touch (ATNA33XC21-0 based)  
**Cams:** 2× front (wide+tele), 2× side/mirrors, 1× rear, 1× DMS (IR)  
**Extras:** radar rear corners (77 GHz), GNSS (+DR), 360° birdseye, predictive suspension preview, LLM/STT/TTS integration

## Goal
- Experimental **Lane Change Assist (LCA)** with high-quality perception and intuitive HMI.
- Tesla-inspired **Qt GUI** running on the Jetson.
- Full sensor pipeline: multi-camera fusion, radar, IMU, GNSS.
- Optional **predictive suspension preview** using ToF/Stereo sensors.

## Big Ideas
- Deterministic time-sync for all cameras (FPD-Link + trigger GPIO).
- Real-time fusion and gap assessment FSM.
- Safety MCU handles watchdog, alerts, and power logic.
- Seamless 360° birdseye integration with the HMI.
- LLM layer for natural voice navigation and media commands.
