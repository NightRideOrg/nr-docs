```mermaid
flowchart TB
  IN[FPD-Link Capture] --> TS[Timestamp Sync/PTP]
  TS --> PER["Perception (DL: lanes/objects)"]
  PER --> TRK[Multi-Object Tracking]
  TRK --> FUS["Fusion (IMU/GNSS/Radar)"]
  FUS --> GAP["Planner (Gap FSM)"]
  GAP --> HMI[Qt HMI]
  GAP --> STM32[Safety Alerts]
  subgraph Voice
    STT[STT] --> LLM[LLM Orchestrator] --> TTS[TTS]
    LLM -->|tool calls| HMI
  end
