### IMU
- 6 or 9 DoF (accelerometer, gyro, magnetometer optional).
- 200 Hz sampling rate, SPI interface.
- Mount near the vehicle’s center of gravity.
- Align sensor axes with vehicle frame; record rotation matrix.

### GNSS
- Dual-band (L1/L5) receiver with active antenna.
- UART interface, 1–10 Hz updates.
- Optional dual-antenna mode for heading estimation.
- PPS output for timestamp synchronization with Jetson.

### Dead Reckoning
- Fuse IMU + GNSS + wheel speed + yaw rate (from CAN).
- EKF or UKF for continuous pose.
- Used for map localization and consistent perception alignment.