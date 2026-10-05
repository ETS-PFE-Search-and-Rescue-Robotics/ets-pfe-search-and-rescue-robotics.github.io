# Handover

## Team lineage

| Cohort | Org | Notes |
|--------|-----|-------|
| **A2025** | `LOG795-UAV-Search-and-Rescue` | Original system |
| **H2026** | `PFE-H2026-Search-and-rescue` | Continued the work |
| **A2026** | `ETS-PFE-Search-and-Rescue-Robotics` | **This org** — consolidated A2025 + H2026 history into one set of repos |

The four repos here (`documentation`, `drone`, `robot`, `ugv_ws`) carry the
merged history of the previous teams. `ugv_ws` has an active
`ros2-humble-develop` branch in addition to `main`.

## Current state

The system **works end to end** (see [Home](index.md) for the demo flow). The
mission for the current cohort is to **harden** it — improve robustness,
precision, and autonomy — not to rebuild it.

## Improvement roadmap (from the professor)

1. **Localization / odometry reliability** — occasional rover position *jumps*
   translate/rotate the map and break the planned path. Find the outlier source,
   add validation/filtering, evaluate wheel + LiDAR + IMU fusion and EKF tuning,
   define drift/stability metrics.
2. **Drone ↔ rover frame sync** — the inverse (drone→rover) transform
   occasionally inverts an axis (rover goes right instead of left). Verify the
   transform matrices both ways, add automated axis-inversion tests, quantify
   the error.
3. **Easier / more automatic calibration** — reduce the manual AprilTag point
   collection, automate it, validate calibration quality, reject outliers, tell
   the operator when it's good enough.
4. **Full-trajectory execution** — the rover currently stops at each waypoint.
   Integrate ROS2 `/navigate_through_poses` with progress tracking,
   failure/timeout handling, cancel/replan, obstacle reaction, and safe recovery.
5. **3D visualization & control UI** — make drone/rover/destination/path clearly
   distinguishable; show system + comms status, real-time progress, errors,
   temperatures; allow safe mission cancel/restart.
6. **QVIO-failure robustness** — detect VIO degradation (low texture, poor light,
   vibration), block use of an invalid map/path, suspend safely, reset affected
   modules, inform the operator.
7. **Thermal constraints** — monitor temperatures, set safe thresholds, evaluate
   cooling (the drone heats up fast when props aren't spinning without external
   cooling, throttling the CPU).

!!! warning "Tentative team priority — not yet official"
    The team's probable main objective is **person-in-distress recognition** (a
    computer-vision capability to detect a person needing rescue — the concrete
    payoff of the search-and-rescue scenario). It is **not** in the professor's
    email and not yet locked in. It would likely build on existing CV in the
    `robot` repo (`cv_ctrl.py`) and `ugv_ws` (`ugv_vision`).

## Recommended startup sequence

Take over the hardware/code/architecture → reproduce the existing demo →
document reproducible problems → design an improved architecture → prioritize by
importance and feasibility → develop incrementally with unit + integration tests
→ validate in a representative search-and-rescue scenario.
