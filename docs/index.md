# ÉTS PFE — Search & Rescue Robotics

A coordinated **aerial drone + ground rover** system for search and rescue,
developed as a final-year engineering project (PFE) at ÉTS.

The drone maps an environment and localizes the rover inside that map; an
operator picks a destination; the system plans a path and sends it to the rover,
which drives there autonomously while avoiding obstacles.

## The end-to-end flow

1. The **drone** flies/moves and builds a 3D map of the area (VOXL QVIO).
2. A **3D web UI** on the drone shows the map, the drone, and the rover.
3. The drone **estimates the rover's pose** in the map (AprilTag + a 3-point
   calibration transform).
4. The operator **chooses a destination**; a path is planned to it.
5. The **nav goal is sent to the rover** over the drone↔rover link.
6. The **rover drives there autonomously** (ROS2 Nav2), avoiding obstacles.

!!! note "Status: continue & harden, not rebuild"
    This pipeline already works end to end. The current cohort's goal is to
    improve its **robustness, precision, and autonomy** — see the
    [Handover](handover.md) page for the roadmap.

## Where to go next

- **[Hardware](hardware.md)** — the drone, the rover, and the name correspondences.
- **[Architecture](architecture.md)** — how the drone and rover talk to each other.
- **[Repositories](repos.md)** — the four repos and how to build/run each.
- **[Handover](handover.md)** — current state, known issues, and the improvement roadmap.

Detailed, repo-specific documentation lives in the
[`documentation`](https://github.com/ETS-PFE-Search-and-Rescue-Robotics/documentation)
repo and in each repo's own README.
