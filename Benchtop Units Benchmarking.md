
Right now I'm using `carter-v24-b8`. I.e. `ssh nvidia@carter-v24-b8` with the usual password.



## Running a benchmark
```
ssh -A nvidia@carter-v24-b8
run_carter_dev_alex
colcon build --symlink-install --packages-up-to isaac_ros_nvblox_nova_benchmark
source install/setup.bash
launch_test src/isaac_ros_benchmark/benchmarks/isaac_ros_nvblox_nova_benchmark/scripts/isaac_ros_3_hawk_1f2l_nvblox_graph.py
```


