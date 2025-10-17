| Sensor             | Rate      | Interface           | Mount                | Purpose                       |
| ------------------ | --------- | ------------------- | -------------------- | ----------------------------- |
| Front Wide Cam     | 20 FPS    | FPD-Link IV → CSI-2 | Windshield center    | Lane detection, near objects  |
| Front Tele Cam     | 20 FPS    | FPD-Link IV → CSI-2 | Windshield center    | Far objects, curve preview    |
| Driver IR Cam      | 20–30 FPS | FPD-Link IV → CSI-2 | Windshield center    | DMS (gaze, attention)         |
| Front Cam          | 20 FPS    | FPD-Link IV → CSI-2 | Front Grill          | Front cross-traffic, parking  |
| Left Mirror Cam    | 20 FPS    | FPD-Link IV → CSI-2 | Left mirror housing  | Blind-spot, adjacent lanes    |
| Right Mirror Cam   | 20 FPS    | FPD-Link IV → CSI-2 | Right mirror housing | Blind-spot, adjacent lanes    |
| Rear Cam           | 20 FPS    | FPD-Link IV → CSI-2 | Tailgate             | Rear cross-traffic, reversing |
| IMU 6-DoF          | 200 Hz    | SPI                 | Near CG, rigid mount | Ego-motion, stability         |
| GNSS Dual-band     | 1–10 Hz   | UART                | Roof or rear shelf   | Position, velocity, heading   |
| 77 GHz Radar (L/R) | 10–20 Hz  | CAN/SPI             | Rear corners         | Relative velocity, distance   |
| ToF or Stereo      | 30–60 Hz  | I²C/USB             | Front grille         | Road profile (for suspension) |

### Notes
- Cameras synchronized via GPIO trigger (shared clock).
- Use consistent lens distortion model and timestamp domain.
- Keep overlapping FOV for fusion and 360° birdseye stitching.
- Calibrate intrinsics and extrinsics early and log results per build.
