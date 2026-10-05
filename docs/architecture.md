# Architecture

## Drone ↔ rover link

The **only** live link between the drone and the rover is UDP (plus a WebSocket
to the UI). Keep both sides in sync — changing a packet format on one side
requires updating the other.

```
                 UDP  <roverIp>:5005  (VIO pose + commands)
   ┌─────────┐  ───────────────────────────────────────▶  ┌─────────┐
   │  DRONE   │                                             │  ROVER  │
   │ client/  │  ◀───────────────────────────────────────  │         │
   └────┬────┘       UDP  :5006  ("ROVER,x,y,o" telemetry)  └─────────┘
        │
        │ WebSocket :8765  (pose/calibration/telemetry push)
        ▼
   ┌─────────┐
   │ 3D WEB  │   HTTP UI on :8080  (+ WebRTC camera via MediaMTX WHEP proxy)
   │   UI    │
   └─────────┘
```

- The drone's `client/main.py` sends drone **VIO + commands** to the rover over
  **UDP `<roverIp>:5005`** (default rover IP `192.168.8.2`).
- It listens for rover telemetry (`ROVER,x,y,o` packets) on **UDP `:5006`** and
  rebroadcasts them to the UI over the **`:8765` WebSocket**.
- The drone serves its control UI over **HTTP `:8080`** and proxies the MediaMTX
  **WHEP** (WebRTC camera) stream so the browser gets video.

## Drone localization

VIO position is read by parsing `voxl-inspect-qvio` stdout (not via ROS2). A
**3-point map calibration** (origin → right → forward) computes a transform that
maps the drone's local frame to world coordinates for the UI and the rover.

## Two rover software stacks, one serial port

The rover has **two** software stacks that both drive the **same ESP32** base
controller over UART — only one may own the serial port at a time:

- **`robot`** — a standalone Flask web-control app (`:5000`), largely vendor code.
- **`ugv_ws`** — the actively developed **ROS2 Humble** SLAM/Nav2 autonomy stack
  (the one used for autonomous navigation).

See [Repositories](repos.md) for how to build and run each.
