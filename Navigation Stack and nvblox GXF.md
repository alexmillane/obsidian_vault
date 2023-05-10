## Basic LiDAR Navigation app
To run the *normal* navigation app, on the host machine:
```bash
./../engine/engine/build/deploy.sh -b bazel -h carter-v23-9.dyn.nvidia.com -d jetpack50 -p <PATH_TO_YOUR_APP>-pkg
```
for example
```bash
./../engine/engine/build/deploy.sh -b bazel -h carter-v23-9.dyn.nvidia.com -d jetpack50 -p //apps/amr/navigation_stack:carter_demo_occupancy_environment_zurich-pkg
```
On the carter:
```bash
cd ~/deploy/<USER>/<APP_YOU_DEPLOYED>
./<PATH_TO_APP_IN_REPO>
```
For example
```bash
cd ~/deploy/alex/carter_demo_occupancy_environment_zurich-pkg/
./apps/amr/navigation_stack/carter_demo_occupancy_environment_zurich
```

## Notes on navigation stack
```mermaid
graph LR

%% main.yaml
id1(robot)

%% robot.subgraph.yaml
id2.1(Robot Interface)
id2.2(Navigation Stack)

%% navigation_stack.subgraph.yaml
id3.1(distance_map)
id3.2(odometry)
id3.3(...)

%% robot_interface_carter_2.<overrides/subgraph>.yaml
id4.1(robot_interface_carter_2)
id4.2(robot_interface_carter_2_with_realsense)

%% robot_interface_carter_2_with_realsense.subgraph.yaml
id5.1(segway)
id5.2(lidar_scan)
id5.3(realsense_front)

%% Distance map overrides
id6.1(distance_map_nvblox.override.yaml)
id6.2(distance_map_egm.override.yaml)

%% CONNECTIONS

%% main -> robot.subgraph.yaml
id1 --> id2.1 & id2.2

%% robot.subgraph.yaml -> navigation_stack.subgraph.yaml
id2.2 --> id3.1 & id3.2 & id3.3

%% navigation_stack.subgraph.yaml -> robot_interface_carter_OVERRIDES
id2.1 -.-> id4.1 & id4.2

%% robot_interface_carter_OVERRIDES -> robot_interface_carter_2_with_realsense.subgraph.yaml
id4.2 --> id5.1 & id5.2 & id5.3

%% distance map overrides
id3.1 -.-> id6.1 & id6.2
```




## Changes I've made
Changes I've made while trying to get nvblox into the navigation stack
* Changed: `robot_interface_carter_2.override.yaml` -> `robot_interface_carter_2_with_realsense.override.yaml`
* Changed: `graphs/single_robot/distance_map_egm.override.yaml` -> `graphs/single_robot/distance_map_nvblox.override.yaml`
* Corrected: ESDF frame parent from `world` to `odom`
* Corrected: mapping frame from `world` to `odom`
* Fixed: Neglected output in the setting of the ESDF frame.


## nvblox + nav-stack Deploy Command
Deploy the navigation stack to the robot
```bash
./../engine/engine/build/deploy.sh -b bazel -h carter-v23-9.dyn.nvidia.com -d jetpack50 -p //apps/amr/navigation_stack:carter_demo_occupancy_nvblox_environment_zurich-pkg
```
