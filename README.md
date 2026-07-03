# Simulatie

Unity simulation for the Jachtseizoen robot swarm. The project visualizes simulated robots, mirrors physical robots from the tracker, and connects to the ROS 2 system through the Unity Robotics ROS-TCP Connector.

## Features

- Unity scene for the Jachtseizoen playing field.
- Simulated runner and hunter robots with potential-field movement.
- Simulated robot pose publishing to ROS 2.
- Game command handling for start, pause, resume and reset.
- Runner visibility handling from the Game Master.
- Physical robot mirroring from camera/tracker pose data.
- ROS-TCP Connector integration for communication with the server stack.

## Repository contents

| Path | Purpose |
|---|---|
| `Assets/Scenes/SampleScene.unity` | Final Unity scene. |
| `Assets/New Folder/RobotBrain.cs` | Simulated robot behaviour and ROS command handling. |
| `Assets/New Folder/publishSimBots.cs` | Publishes simulated robot poses. |
| `Assets/New Folder/spawnPhysicalBots.cs` | Mirrors physical robots from tracker positions. |
| `Assets/New Folder/CameraManager.cs` | Camera handling. |
| `Assets/New Folder/followCamera.cs` | Follow camera behaviour. |
| `Assets/New Folder/robotControl.cs` | Manual robot control/test script. |
| `Assets/New Folder/wheelEncoder.cs` | Wheel encoder test publisher. |
| `Packages/manifest.json` | Unity package dependencies. |
| `ProjectSettings/ProjectVersion.txt` | Unity editor version. |

## Getting started

### Requirements

- Unity `6000.4.5f1`
- Unity Robotics ROS-TCP Connector package, already listed in `Packages/manifest.json`
- Running ROS-TCP Endpoint from the server stack

### Install

1. Clone the repository.
2. Open the project folder in Unity Hub with Unity `6000.4.5f1`.
3. Open `Assets/Scenes/SampleScene.unity`.
4. Select the ROSConnection object in the scene and confirm the ROS server address and port.
5. Press Play.

The current scene is configured to connect to `thomasenco.com` on TCP port `10000`.

## Configuration

The values below are the current repository values.

| Setting | Current value | Effect |
|---|---:|---|
| Unity editor | `6000.4.5f1` | Unity version used by the project. |
| Scene | `Assets/Scenes/SampleScene.unity` | Final scene. |
| ROS IP address | `thomasenco.com` | ROS-TCP Endpoint host configured in the scene. |
| ROS port | `10000` | ROS-TCP Endpoint port configured in the scene. |
| Connect on start | enabled | Unity connects to ROS when Play Mode starts. |
| Network timeout | `2` seconds | ROS-TCP Connector timeout. |
| Keepalive time | `1` second | ROS-TCP Connector keepalive. |
| PublishSimBots publish rate | `0.0166` seconds in the scene | Interval for publishing simulated robot poses. |
| Simulated robot IDs | `101`, `102`, `103`, `104` | IDs used by RobotBrain objects in the final scene. |
| RobotBrain `testDrive` | disabled | Manual target-drive test mode. |
| Runner speed | `1.0` | Simulated runner movement speed. |
| Hunter speed | `1.0` | Simulated hunter movement speed. |
| Goal attraction strength | `2.0` | Pull toward the selected target. |
| Robot repulsion strength | `4.0` | Push away from other robots. |
| Robot influence radius | `2.5` | Distance at which robot repulsion starts. |
| Wall repulsion strength | `3.0` | Push away from arena edges. |
| Wall safety margin | `1.0` | Distance from wall where wall repulsion starts. |
| Jitter strength | `0.3` | Random movement nudge strength. |
| Jitter interval | `0.2` seconds | Interval for refreshing jitter direction. |
| Runner corner interval | `5` seconds | Runner patrol target switch interval. |
| Hunter flee radius | `4.1` | Distance at which runner starts fleeing from hunters. |

## Actions

| Action | Method |
|---|---|
| Start simulation | Press Play in Unity with `SampleScene` open. |
| Stop simulation | Stop Play Mode. |
| Change ROS endpoint | Edit the ROSConnection object in the scene. |
| Run isolated robot movement test | Enable `testDrive` on a RobotBrain object and set `testTarget`. |
| Watch ROS connection state | Use the ROS-TCP Connector HUD enabled in the scene. |

The main game is controlled by the Game Master. Unity reacts to Game Master commands instead of providing the primary operator controls.

## Calibration

Unity does not perform camera calibration. The simulation uses the shared project coordinate model where Unity X maps to ROS X and Unity Z maps to ROS Y. Physical robot calibration is handled by the Tracking Module.

## Connections

| Direction | Interface | Purpose |
|---|---|---|
| Outgoing | ROS-TCP Connector to server | Connects Unity to the ROS 2 system. |
| Outgoing | `/unity/pos` (`geometry_msgs/msg/PoseArray`) | Publishes simulated robot positions. |
| Outgoing | `/game/robots/ready` (`std_msgs/msg/Int32`) | Simulated robots announce that they are ready. |
| Incoming | `/robots/pos` (`geometry_msgs/msg/PoseArray`) | Shared robot position input used by RobotBrain. |
| Incoming | `/game/command` (`std_msgs/msg/String`) | Start, pause, resume and reset commands. |
| Incoming | `/robots/seen` (`std_msgs/msg/Bool`) | Runner visibility state. |
| Incoming | `/cam/pos` (`geometry_msgs/msg/PoseArray`) | Physical robot positions for scene mirroring. |
| Prototype/test | `/cmd_vel`, `/wheel_encoder_left`, `/wheel_encoder_right` | Manual/test scripts, not part of the main final game flow. |
