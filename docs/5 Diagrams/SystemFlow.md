
```mermaid
flowchart LR
  CAMS[Multi-Cam Ingest] --> SYNC[Sync/PTP]
  SYNC --> PER[Perception]
  PER --> FUS[Fusion/Tracking]
  FUS --> PLAN[Planner]
  PLAN --> HMI[Qt HMI]
  PLAN --> STM32[Safety MCU]
