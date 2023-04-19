#ros #segmentation

## Generating TensorRT files on NGC
So when you download models from NGC they are encrypted weights. You need to generate a tensorRT file using the TAO converter. This is the process followed in the quick start guide on isaac_ros_image_segmentation page.

Note that I'm running in a version of the workspace which contains the public versions of all the repos.

### Dirty Notes
My attempt at getting a bigger network going. Note that
- The input name changed so the commands below reflect that
- The model they point to on the README didn't work for me, so I used a different one which is quantized [here](https://catalog.ngc.nvidia.com/orgs/nvidia/teams/tao/models/peoplesemsegnet/files?version=deployable_quantized_vanilla_unet_v2.0). 

Model download:
```
mkdir -p /tmp/models/peoplesemsegnet/1
cd /tmp/models/peoplesemsegnet
wget 'https://api.ngc.nvidia.com/v2/models/nvidia/tao/peoplesemsegnet/versions/deployable_quantized_vanilla_unet_v2.0/files/peoplesemsegnet_vanilla_unet_dynamic_etlt_int8.cache'
wget 'https://api.ngc.nvidia.com/v2/models/nvidia/tao/peoplesemsegnet/versions/deployable_quantized_vanilla_unet_v2.0/files/peoplesemsegnet_vanilla_unet_dynamic_etlt_int8_fp16.etlt'
```

Quantization command:
```
/opt/nvidia/tao/tao-converter -k tlt_encode -d 3,544,960 -p input_1:0,1x3x544x960,1x3x544x960,1x3x544x960 -t int8 -c peoplesemsegnet_vanilla_unet_dynamic_etlt_int8.cache -e /tmp/models/peoplesemsegnet/1/model.plan -o argmax_1 peoplesemsegnet_vanilla_unet_dynamic_etlt_int8_fp16.etlt
```

Config copy
```
cp /workspaces/isaac_ros-dev/src/isaac_ros_image_segmentation/resources/peoplesemsegnet_shuffleseg_config.pbtxt /tmp/models/peoplesemsegnet/config.pbtxt
```

Modify the config to rename the model name and the input name such that it looks like
```
name: "peoplesemsegnet"
platform: "tensorrt_plan"
max_batch_size: 0
input [
  {
    name: "input_1:0"
    data_type: TYPE_FP32
    dims: [ 1, 3, 544, 960 ]
  }
]
output [
  {
    name: "argmax_1"
    data_type: TYPE_INT32
    dims: [ 1, 544, 960, 1 ]
  }
]
version_policy: {
  specific {
    versions: [ 1 ]
  }
}
```

Launch:
```
ros2 launch isaac_ros_unet isaac_ros_unet_triton.launch.py model_name:=peoplesemsegnet model_repository_paths:=['/tmp/models'] input_binding_names:=['input_1:0'] output_binding_names:=['argmax_1'] network_output_type:='argmax'
```

Play the dataset
```
ros2 bag play rosbag2_2022_11_29-16_41_57 -l --qos-profile-overrides-path qos_overrides.yaml
```

## Commands for cloning into container workspace

>Note: Currently I am doing all this stuff in a public version of the docker, which I clone specifically for this stuff. Would be good to get everything going in the private version.


Modifications for cloning the model into the workspace such that it is preserved from restarts of the container.

```
mkdir -p models/peoplesemsegnet/1
cd models/peoplesemsegnet

wget 'https://api.ngc.nvidia.com/v2/models/nvidia/tao/peoplesemsegnet/versions/deployable_quantized_vanilla_unet_v2.0/files/peoplesemsegnet_vanilla_unet_dynamic_etlt_int8.cache'

wget 'https://api.ngc.nvidia.com/v2/models/nvidia/tao/peoplesemsegnet/versions/deployable_quantized_vanilla_unet_v2.0/files/peoplesemsegnet_vanilla_unet_dynamic_etlt_int8_fp16.etlt'

/opt/nvidia/tao/tao-converter -k tlt_encode -d 3,544,960 -p input_1:0,1x3x544x960,1x3x544x960,1x3x544x960 -t int8 -c peoplesemsegnet_vanilla_unet_dynamic_etlt_int8.cache -e 1/model.plan -o argmax_1 peoplesemsegnet_vanilla_unet_dynamic_etlt_int8_fp16.etlt

cp /workspaces/isaac_ros-dev/src/isaac_ros_image_segmentation/resources/peoplesemsegnet_shuffleseg_config.pbtxt config.pbtxt
```

Modify the config to rename the model name and the input name such that it looks like
```
name: "peoplesemsegnet"
platform: "tensorrt_plan"
max_batch_size: 0
input [
  {
    name: "input_1:0"
    data_type: TYPE_FP32
    dims: [ 1, 3, 544, 960 ]
  }
]
output [
  {
    name: "argmax_1"
    data_type: TYPE_INT32
    dims: [ 1, 544, 960, 1 ]
  }
]
version_policy: {
  specific {
    versions: [ 1 ]
  }
}
```

Launch:
```
ros2 launch isaac_ros_unet isaac_ros_unet_triton.launch.py model_name:=peoplesemsegnet model_repository_paths:=['/workspaces/isaac_ros-dev/models'] input_binding_names:=['input_1:0'] output_binding_names:=['argmax_1'] network_output_type:='argmax'

ros2 bag play rosbag2_2022_11_29-16_41_57 -l --qos-profile-overrides-path qos_overrides.yaml
```

## Re-record a Bag with segmentation
I am re-recording a bag with the segmentation masks in for remo to play around in 3D.
```
ros2 bag record \
	/camera/color/camera_info \
	/tf_static \
	/camera/realsense_splitter_node/output/depth \
	/camera/realsense_splitter_node/output/infra_2 \
	/camera/realsense_splitter_node/output/infra_1 \
	/visual_slam/tracking/odometry \
	/camera/realsense_splitter_node/output/pointcloud \
	/camera/color/image_raw \
	/tf \
	/visual_slam/status \
	/visual_slam/vis/observations_cloud \
	/camera/depth/camera_info\
	/camera/infra1/camera_info \
	/visual_slam/vis/landmarks_cloud \
	/camera/infra2/camera_info \
	/unet/colored_segmentation_mask \
	/unet/raw_segmentation_mask
```
