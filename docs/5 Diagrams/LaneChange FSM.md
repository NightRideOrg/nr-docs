
```mermaid
stateDiagram-v2
  [*] --> Idle
  Idle --> Check: Turn signal on
  Check --> Wait: No gap
  Check --> Execute: Gap OK
  Execute --> Monitor
  Monitor --> Idle: Completed
  Monitor --> Abort: Conflict