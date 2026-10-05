# Repositories

Four repos make up the project. Detailed instructions live in each repo's README;
this is a quick-reference.

## [documentation](https://github.com/ETS-PFE-Search-and-Rescue-Robotics/documentation)

Setup guides, hardware notes, and incident reports (`material/`, `setup/`,
`software/`, `incident-reports/`). Start here for connection details and
hardware correspondences.

## [drone](https://github.com/ETS-PFE-Search-and-Rescue-Robotics/drone)

VOXL 2 / Starling 2 Max drone. The primary deliverable is `client/` — a pure
Python standard-library web server that runs **on the drone** (no framework). It
serves the 3D control UI, proxies the WebRTC camera, and bridges drone position
to the rover.

```bash
cd client
python3 main.py            # --roverIp / -ri <ip>  (default 192.168.8.2)
```

UI on `:8080`, WebSocket on `:8765`. The drone is **offline**, so deployment is
an offline bundle (`send_to_drone.sh`).

## [robot](https://github.com/ETS-PFE-Search-and-Rescue-Robotics/robot)

Waveshare UGV standalone **Flask** web-control app (MJPEG/WebRTC video, a web
CLI, SocketIO command dispatch to the ESP32).

```bash
sudo ./setup.sh            # deps + venv
./autorun.sh               # @reboot crontab that launches app.py (run WITHOUT sudo)
```

Serves on `:5000`.

## [ugv_ws](https://github.com/ETS-PFE-Search-and-Rescue-Robotics/ugv_ws)

The rover's **ROS2 Humble** colcon workspace — SLAM, Nav2, teleop, AprilTag
vision. Development and builds happen **inside a Docker container** on the Jetson.

```bash
./ros2_humble.sh           # start + exec into the container
./build_common.sh          # rebuild + source install/setup.bash

# then, inside the container:
ros2 launch ugv_bringup bringup_lidar.launch.py use_rviz:=true
ros2 launch ugv_slam gmapping.launch.py use_rviz:=true
ros2 launch ugv_nav nav.launch.py use_rviz:=true
```

!!! warning "Path inconsistencies"
    Some scripts hard-code `/home/jetson/ugv_ws` while others say
    `/home/ws/ugv_ws`; the `robot` boot scripts hard-code `~/ugv_jetson/` though
    the checkout is named differently. Reconcile paths before relying on boot
    autorun — see the repo READMEs.
