# yikun_msgs

ROS 1 (catkin) messages, services, and actions for the Yikun patrol robot.

`modbus_interface` calls the `yikun_msgs/ModbusCmd` service. [moma-patrol-system](https://github.com/Zeuss-0/moma-patrol-system-1-) vendors this repository as the submodule `3rd/yikun_msgs`.

## Layout

The catkin package is the inner `yikun_msgs/` directory. The git root holds this README, the license, and `.gitignore`.

```text
yikun_msgs/                  repo root (submodule checkout)
├── README.md
├── LICENSE
└── yikun_msgs/              catkin package name: yikun_msgs
    ├── package.xml
    ├── CMakeLists.txt
    ├── msg/
    ├── srv/
    └── action/
```

Catkin discovers packages by walking the workspace and reading `package.xml`. With moma-patrol-system checked out as the workspace `src/` tree, the package file is:

```text
src/3rd/yikun_msgs/yikun_msgs/package.xml
```

Leave the repo root without its own `package.xml` and without a `CATKIN_IGNORE` file. Catkin already finds the inner package. A `CATKIN_IGNORE` next to this README would hide it. The nested directory stays where it is so existing submodule checkouts keep the same paths.

Generated headers follow the **package name**, not the extra directory. Dependents include them as usual:

```cpp
#include "yikun_msgs/ModbusCmd.h"

bool command_callback(yikun_msgs::ModbusCmdRequest &req,
                      yikun_msgs::ModbusCmdResponse &res);
```

## Build

ROS 1 with catkin (tested on Noetic) and a catkin workspace. From a workspace that already has this repo as `src/3rd/yikun_msgs`:

```bash
cd <your_workspace>
rosdep install --from-paths src --ignore-src -y
catkin_make
source devel/setup.bash
```

To build this repository by itself:

```bash
mkdir -p ~/catkin_ws/src
git clone https://github.com/Zeuss-0/yikun_msgs.git ~/catkin_ws/src/yikun_msgs
cd ~/catkin_ws
rosdep install --from-paths src --ignore-src -y
catkin_make --pkg yikun_msgs
source devel/setup.bash
```

`rossrv show yikun_msgs/ModbusCmd` should then list the service.

`modbus_interface` declares `yikun_msgs` as a build and run depend. Build this package first (or let catkin order the two packages in one workspace). Its command server uses:

| Field | Meaning |
| --- | --- |
| `cmd` | `1` read DI, `2` set DO, `3` wait for DI |
| `number` | IO index, starting at 0 |
| `value` | `0` or `1` when `cmd` is 2 or 3 |
| `duration` | reserved wait time |
| `result`, `description` | response |

## Interfaces

Messages, services, and actions registered in `yikun_msgs/CMakeLists.txt` are what catkin generates. Grouped by what they cover:

- **Tasks and state.** `Application`, `AppStatus`, `AppUpdate`, `ListTasks`, `StartTask`, `StopTask`, `StatusUpdate`, `TaskPojo`, `SetState`, `GetState`, `ControlPojo`, `ControllerSwitch`.
- **Map and localization.** `Ismapping`, `Isreading`, `Localization`, `Location`, `SaveMap`, `SetPose`, `MapSave`.
- **Motion, dock, and paths.** `Dock`, `DockPose`, `RelativeMove`, `AutoDock`, `Dock`, `MoveBase`, `PosCss`, `IndustralPath`, `IndustralPathTask`, `IndustralPathCss`, `PointAlignment`, `SearchOptimalWayTask`, `BestWayForWeb`, `BestWayFeedback`.
- **Devices.** `ModbusCmd`, `URCommand`, `SafetyLaser`, `ConfidenceLevel`, `SetWifi`, `Wifi`, `WifiList`, `ComWithWeb`, `CameraControl`, `TakePictures`, `PutterPosition`, `AiHttpPost`.
- **Navigation types** adapted from upstream ROS (see `NOTICE`). `VoxelGrid`, `SBPLLatticePlannerStats`, `FeedbackMsg`, `ObstacleMsg`, `TrajectoryMsg`, `TrajectoryPointMsg`.
- **Other.** `Num`, `Image`, `LineSegment`, `LineSegmentList`.

Three service files are on disk and are **not** generated:

| File | Why it is skipped |
| --- | --- |
| `srv/CameraPosition.srv` | Empty. Adding it to `add_service_files` would fail generation. |
| `srv/RobotAction.srv` | Not listed in `CMakeLists.txt`. |
| `srv/OsiTaskCtrlBean.srv` | Not listed in `CMakeLists.txt`. |

This change leaves them unregistered, so the generated API stays the same.

## License

[Apache License 2.0](LICENSE), copyright 2019 Shanghai Yikun Electrical Engineering Co., Ltd. `modbus_interface` (same maintainer, `sjy@todo.todo`) already declares Apache 2.0 and carries that copyright in its sources. `wj_716N_lidar` uses the same license. See `NOTICE` for navigation messages adapted from BSD-licensed ROS packages.
