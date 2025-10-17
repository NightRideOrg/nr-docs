```mermaid
flowchart LR
  subgraph SENSORS
    FW[Front Wide Cam 30 FPS]
    FT[Front Tele Cam 30 FPS]
    ML[Left Mirror Cam]
    MR[Right Mirror Cam]
    RE[Rear Cam]
    DMS[Driver IR Cam]
    IMU[IMU 200 Hz]
    RAD[77 GHz Radar L/R]
    GNSS["GNSS 1–10 Hz (+DR)"]
  end

  subgraph JETSON[Jetson AGX Orin]
    ING[FPD-Link Capture + Sync]
    PER["Perception (lanes/objects)"]
    TRK[Tracking]
    FUS["Fusion (IMU/GNSS/Radar)"]
    GAP[Planner / Gap FSM]
    BEV[360 Birdseye]
    HMI[Qt HMI 1080p/60]
    VOX[LLM / STT / TTS]
  end

  subgraph SAFETY[STM32 MCU]
    WD[Watchdog]
    CANF[CAN Gateway + Filters]
    ACT[Haptic / LED Alerts]
  end

  FW-->ING; FT-->ING; ML-->ING; MR-->ING; RE-->ING; DMS-->ING
  ING-->PER-->TRK-->FUS-->GAP
  BEV-->HMI
  GAP-->HMI
  GAP-->SAFETY
  IMU-->FUS; GNSS-->FUS; RAD-->FUS
  VOX-->HMI
