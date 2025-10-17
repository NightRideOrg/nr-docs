## 360° Birdseye

**Capture**
- Ensure overlapping FOV at corners; fixed exposure if possible.

**Calibration**
1) Intrinsics per cam (chessboard/AprilGrid).  
2) Extrinsics to car frame (board around car).  
3) Fit planar homography for ground for each cam.  
4) Build seam masks; choose stitch order (minimize parallax seams).

**Runtime**
- Warp cameras via homographies (GPU shader).
- Composite with masks; optional tone match between cams.
- Target 60 FPS; precompute LUTs for speed.

**Diagnostics**
- Grid overlay on ground plane; check straight lines.
