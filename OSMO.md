# OSMO user guide

https://us-west-2-aws.osmo.nvidia.com/docs/index.html

## NGC URL
https://catalog.ngc.nvidia.com/

# Run nvblox core benchmark
```
osmo workflow submit replica.yaml --set cpu=8 os=linux platform_type=x86 memory=100Gi disk=600Gi image=nvcr.io/nvstaging/isaac-amr/nvblox-nightly-x86_64:latest workflow_name=nvblox_replica_testing kpi_namespace=nvblox/core/aarch64/testing
```
> Note that you have to remove the ssa secrets line from the workflow file in order to run it yourself.
# OSMO datasets
Upload dataset (here replica):
```bash
osmo dataset upload replica replica
```

## Swiftstack web interface
https://cssportal.sre.nsv.nvidia.com:4443/


## nvblox Image Upload
Build and upload a docker image
```bash
src/nvblox_tools/docker_utils/build_nvblox_docker_image.sh -t alex_testing -p
```

Build docker and dont push
```bash
src/nvblox_tools/docker_utils/build_nvblox_docker_image.sh -t alex_testing
```

# OSMO Datasets
Dataset login is different to the normal OSMO login. Instructions can be found here:
```bash
https://docs.google.com/document/d/1CWPEh3JP80KAjp2jyx1D--2UtbdsPpFGHx2vjiQ7qn0/edit#heading=h.braygt81wr5u
```
The dashboard for storage can be found [here](https://cssportal.sre.nsv.nvidia.com:4443/user/(userRouter:home). Which is also where you get the stuff for the datasets login.

## Working with a OSMO Dataset
Check out a datasets files
```
osmo dataset inspect isaac_ros_nvblox_tests
```

Add a new folder to an existing dataset
```
osmo dataset update isaac_ros_nvblox_tests --add /home/alex/workspaces/carter-dev/ros_ws/src/
isaac_ros_nvblox/nvblox_test/test/test_cases/2024_01_31_with_obstacles_cut
```

Delete some files
```
osmo dataset update isaac_ros_nvblox_tests --remove zanker_2_cut/rosbag2_2024_02_01-11_42_39_0.mcap
```
# Run OSMO Docker locally
These commands bring down the docker from NGC so you can test in it.

Run docker with datasets mounted:
```bash
docker run -it -v /home/alex/datasets/replica:/datasets/replica nvcr.io/nvstaging/isaac-amr/nvblox_isaac_ros:alex_testing
```

Run Replica inside local docker:
```bash
./src/isaac_ros_nvblox/nvblox/nvblox/integration_tests/replica/replica_reconstruction_test.py /datasets/replica/office0 --fuse_replica_binary_path install/nvblox/bin/nvblox/fuse_replica
```
  

# Submit a workflow (perf)
```
osmo workflow submit nvblox_tools/workflows/perf_workflow.yaml
```

# GUI to view containers
https://ngc.nvidia.com/containers/nvstaging:isaac-amr:nvblox_isaac_ros/tags


# OSMO logs
https://osmo.nvidia.com/workflow/logs/

  

# Confusing terms

**NGC:** Is the docker image repo
**Maglev:** Is the backend that launches the jobs and makes it possible to connect on it etc
**Swiftstack:** Is the data lake
**OSMO:** Is the abstraction layer over maglev
**nvcr.io:** WUT?!

  
  
  
  
  

# Re-signup to data
Storage namespace
pass: Unsecure1datadatadata