# Hardware

## Name correspondences

The docs and code use several names for the same physical things. This table is
the Rosetta Stone (mirrors the one in the `documentation` repo):

| Name | Role |
|------|------|
| Starling (2 Max) | Drone airframe |
| VOXL (2) | Companion computer on the drone (ModalAI) |
| UGV | The ground rover (Waveshare) |
| Jetson | Main computer on the rover |
| ESP32 | Sub-controller on the rover (motors + sensors) |

!!! info "Jetson, not Raspberry Pi"
    Some code and comments say "Raspberry Pi", but the serial paths
    (`/dev/ttyTHS*`), `jtop`, and the `~/ugv_jetson/` paths show the rover runs on
    an **NVIDIA Jetson**.

## Drone (Starling 2 Max / VOXL 2)

- Runs **offline** — code is deployed as an offline bundle and run on-device.
- Localization uses **VOXL QVIO** (visual-inertial odometry).
- voxl-suite is pinned to **1.5.1** (QVIO stops working with the cameras on newer
  versions).

## Rover (Waveshare UGV on Jetson + ESP32)

- The **Jetson** runs the autonomy software (two possible stacks — see
  [Repositories](repos.md)).
- The **ESP32** is the lower-level controller: it drives the motors and gimbal
  and reads sensors, speaking **115200-baud JSON-over-UART** (messages keyed by
  `"T"`). It also parses the LD-series LiDAR.

## Access

Connection details (Wi-Fi SSIDs, SSH logins, IPs) are documented in the
individual repo READMEs and the `documentation` repo — they are **not** repeated
here. See:

- [drone README](https://github.com/ETS-PFE-Search-and-Rescue-Robotics/drone)
- [documentation repo](https://github.com/ETS-PFE-Search-and-Rescue-Robotics/documentation)
