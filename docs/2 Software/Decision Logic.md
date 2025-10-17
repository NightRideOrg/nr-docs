```mermaid
stateDiagram-v2
  [*] --> Idle
  Idle --> Check: Turn signal on
  Check --> Wait: Gap unsafe (time headway < threshold)
  Check --> Reserve: Gap safe & speed delta OK
  Reserve --> Execute: Driver confirms/commit
  Execute --> Monitor: Track adjacent vehicles
  Monitor --> Idle: Completed
  Monitor --> Abort: Obstacle / deltaV too high
