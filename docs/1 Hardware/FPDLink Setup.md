### Components
- **Deserializer:** DS90UB960 (4 ports)
- **Serializers:** DS90UB953 (per camera)
- **Cables:** Coax (RG-174) or Shielded Twisted Pair (STP)
- **PoC:** Integrated on each link (≈3–4 W per cam)

### Sync & Timing
- 30 FPS per camera target.
- HW frame-sync via GPIO forwarding or external trigger fan-out.
- Deterministic latency target <50 ms camera → Jetson frame.
- Use identical exposure settings across cameras for fusion consistency.

### Bandwidth / Line Rates
| Interface | Typical Bandwidth |
|------------|------------------:|
| FPD-Link IV | 4–6 Gbps |
| MIPI CSI-2 | 4 lanes × 2.5 Gbps per port |

### Integration Notes
- Calibrate trigger offsets to minimize frame skew.
- Ensure coax connectors are crimped and shielded properly.
- Active cooling for deserializer boards recommended.
- Keep deserializers close to Jetson (short CSI lines).