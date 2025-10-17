## 📷 Camera Configuration Overview

| Camera              | Mount                     | Purpose                    | Used by OpenPilot        | Used by BEV | Used by Dashcam |
| ------------------- | ------------------------- | -------------------------- | ------------------------ | ----------- | --------------- |
| **Windshield Wide** | Top center                | Main forward view          | ✅ (ROAD_CAM or WIDE_CAM) | Optional    | ✅               |
| **Windshield Tele** | Top center                | Far detection              | ✅ (ROAD_CAM or WIDE_CAM) | Optional    | ✅               |
| **Front Grill**     | Front bumper              | BEV front overlap, parking | ❌                        | ✅           | ✅               |
| **Mirror Left**     | Mirror housing            | Blind spot / left BEV      | ❌                        | ✅           | ✅               |
| **Mirror Right**    | Mirror housing            | Blind spot / right BEV     | ❌                        | ✅           | ✅               |
| **Rear Cam**        | Tailgate                  | Rear BEV + reversing view  | ❌                        | ✅           | ✅               |
| **Driver IR Cam**   | Headliner (facing driver) | Driver Monitoring          | ✅ (DRIVER_CAM)           | ❌           | ✅               |

---

## 🎯 Frame Rate & Timing Strategy

### Core rule

OpenPilot’s models expect **exactly one frame every 50 ms → 20 FPS**.  
→ Do **not** use 30 FPS (drifts vs model tick).  
→ **60 FPS** is acceptable **if you downsample deterministically to 20 FPS** (pick newest frame ≤ tick time).

### Recommended FPS per camera

| Camera          | Target FPS                   | Justification                            |
| --------------- | ---------------------------- | ---------------------------------------- |
| Windshield Wide | **20 FPS** or **60 → 20**    | Matches OpenPilot model (50 ms tick)     |
| Windshield Tele | **20 FPS** or **60 → 20**    | Same cadence as Wide                     |
| Driver IR       | **20–30 FPS (→ 20 aligned)** | DMS doesn’t need >30 FPS, easier to sync |
| Front Grill     | **20 FPS**                   | Sync with BEV system                     |
| Mirror L/R      | **20 FPS**                   | Synchronize BEV stitching                |
| Rear            | **20 FPS**                   | Consistent BEV frame rate                |

---

## 🧩 OpenPilot Camera Mapping

```bash
# Example mapping (adjust /dev/videoX to match your system)
USE_WEBCAM=1 ROAD_CAM=2 WIDE_CAM=3 DRIVER_CAM=1 system/manager/manager.py
```

### Stable device assignment (udev example)

Create `/etc/udev/rules.d/99-cams.rules`:

```
SUBSYSTEM=="video4linux", ATTRS{serial}=="ABC123", SYMLINK+="cam_road"
SUBSYSTEM=="video4linux", ATTRS{serial}=="DEF456", SYMLINK+="cam_wide"
SUBSYSTEM=="video4linux", ATTRS{serial}=="GHI789", SYMLINK+="cam_driver"
```

Then you can launch with:

```bash
USE_WEBCAM=1 ROAD_CAM=/dev/cam_road WIDE_CAM=/dev/cam_wide DRIVER_CAM=/dev/cam_driver system/manager/manager.py
```

---

## 🕒 Synchronization

- Use a **global 50 ms scheduler** (0 ms, 50 ms, 100 ms, …).
    
- Each camera writes frames with **hardware timestamps**.
    
- At every tick, pick the **latest frame ≤ tick time** (max age ≤ 25 ms).
    
- If using DS90UB960 / FPD-Link IV:  
    → Use GPIO FSIN trigger forwarding for hardware frame sync.  
    → Match exposure start across all BEV cameras.
    

---

## 🐦 Bird’s-Eye View (BEV) Setup

|Camera|Role|
|---|---|
|Front Grill|Forward coverage|
|Mirror Left|Left coverage|
|Mirror Right|Right coverage|
|Rear|Rear coverage|
|(Optional) Windshield Wide|Extra overlap|

### BEV pipeline @ 20 FPS

1. Calibrate intrinsics/extrinsics → vehicle coordinate system.
    
2. Compute ground-plane homographies.
    
3. GPU warp (LUT or fragment shader).
    
4. Seam masks + gain correction.
    
5. Compose final BEV frame.
    
6. Optionally display at **60 FPS UI**, but use **20 FPS inputs**.
    

---

## 🎥 Dashcam Recording

- Record all cameras simultaneously (H.265 / HEVC).
    
- **Resolution:** 1080p @ 20 FPS.
    
- **Bitrate:** 4–8 Mbit/s (day) / 8–12 Mbit/s (night).
    
- 6 cams × ~8 Mbit/s ≈ 48 Mbit/s (6 MB/s).
    
- 1 TB SSD ≈ 45–50 hours continuous footage.
    
- Store raw streams + JSON metadata (timestamp, camera, exposure, GPS).
    

---

## 📡 Data Rates

|Format|1080p @ 20 FPS|1080p @ 60 FPS|
|---|---|---|
|RAW12|~50–60 MB/s per cam|~150–180 MB/s per cam|
|H.265 compressed|4–10 MB/s per cam|— (usually not stored at 60 FPS)|

---

## 🧠 Implementation Tips

- If a sensor defaults to 60 FPS, check for a real 20 FPS PLL mode (adjust blanking if needed).
    
- For Jetson: use `nvarguscamerasrc` (CSI) or `v4l2src` (USB) with `NVMM` zero-copy.
    
- Sync timestamps via `CLOCK_MONOTONIC_RAW` or PTP (Chrony).
    
- Use ringbuffers ≥ 8 frames per camera to survive scheduler jitter.
    
- Keep exposure & gain identical for BEV cams to avoid color seams.
    

---

## ⚡ TL;DR

- All perception & BEV inputs: **20 FPS (50 ms tick)**
    
- OpenPilot: **Windshield Wide + Tele + Driver**
    
- Dashcam: **all cams @ 20 FPS H.265**
    
- BEV: **Front Grill + Mirrors + Rear** (20 FPS input, 60 FPS UI optional)
    
- Use deterministic resampling if 20 FPS mode not available
    
- Sync everything on a 50 ms global clock
    

---

Would you like me to add a **C++/Python frame-resampler snippet** next (that aligns multiple 60 FPS inputs to a single 20 FPS timeline)?