## HMI (Qt 6 on Jetson)

- 1920×1080 @60 FPS target; dark theme; GPU-accelerated (EGL).
- Overlays: lane/path, objects, blind-spot arrows, speed, alerts.
- Metrics popup: FPS, temps, dropped frames, CAN error counters.
- Touch gestures + voice confirmations.

**Run tips (Wayland/X11)**
```bash
QT_XCB_GL_INTEGRATION=xcb_egl \
QT_WAYLAND_CLIENT_BUFFER_INTEGRATION=xcomposite-egl \
./Car_Infotainment
