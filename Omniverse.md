## Server URL
```
omniverse://ov-isaac-dev.nvidia.com/
```


## Isaac Sim Humans example

Command to launch Isaac Sim 
```bash
omni_python ~/workspaces/isaac_ros-dev/src/isaac_ros_nvblox/nvblox_examples/nvblox_isaac_sim/omniverse_scripts/start_isaac_sim.py     --with_people --scenario_path=/Isaac/Samples/NvBlox/carter_warehouse_navigation_with_people.usd --/persistent/isaac/asset_root/default="omniverse://ov-isaac-dev.nvidia.com"
```

Command to launch nvblox_ros
```bash
ros2 launch nvblox_examples_bringup isaac_sim_example.launch
```