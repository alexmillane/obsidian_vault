
Useful links:
* [Confluence docs for the data recorder](https://confluence.nvidia.com/pages/viewpage.action?spaceKey=RPT&title=NOVA+Data+Recording+and+Processing+Guide)
* [Isaac 2.0 EA Documentation](https://docs.google.com/document/d/1Qk0CzmSHjxryREHgF6p-W8YaFu4crf0n4AyfcPdkJMg/edit#)

Things I did to get it working (whether it is fully "working" yet remains to be seen)
* `sudo apt-get install nova-init`
* Create the aws config file in the home directory as specified [here](https://docs.google.com/document/d/1Qk0CzmSHjxryREHgF6p-W8YaFu4crf0n4AyfcPdkJMg/edit#heading=h.dyz0fqqls6c6)
* Created a `recordings` folder in `/media/orin`
* Actually scratch the above command. That folder was on the soldiered on storage so I changed it to saving to the SSD and changed the launch command (see below).

## Launch Command
The launch command which works for me:
```bash
docker run -it --rm --privileged --network=host -e AWS_CONFIG_FILE=/etc/aws/config -e AWS_SHARED_CREDENTIALS_FILE=/etc/aws/credentials \
  --mount type=bind,source=/dev,target=/dev \
  --mount type=bind,source=/etc/nova,target=/etc/nova \
  --mount type=bind,source=$HOME/.aws,target=/etc/aws \
  --mount type=bind,source=/sys/devices,target=/sys/devices \
  --mount type=bind,source=/tmp/argus_socket,target=/tmp/argus_socket \
  --mount type=bind,source=/sys/bus/iio/devices,target=/sys/bus/iio/devices \
  --mount type=bind,source=/home/nvidia/recordings,target=/media/orin_ssd/recordings \
  --mount type=bind,source=/var/nvidia/nvcam/settings/camera_overrides.isp,target=/var/nvidia/nvcam/settings/camera_overrides.isp \
  nvcr.io/nvstaging/isaac-amr/apps_amr_data_recorder_data_recorder:master-aarch64
```
Things I've changed from the launch command in confluence:
* Removed the last param regarding aws crap
* Changed the location of the place to save

## Playback command
We run the `data_replayer` as:
```bash
bazel run //apps/amr/data_recorder:data_replayer -- --param=replayer.pod_replayer=file=file_path=<path>
```

# Troubleshooting

Things I've found
* If the data recorder starts crashing because of hawk run: `sudo service nvargus-daemon restart`
* 

# Notes made during attempt
A few notes:
* Note that the launch command links `.aws` to `/etc/aws` which initially had me confused.

The command on the confluence page is:
```bash
docker run -it --rm --privileged --network=host -e AWS_CONFIG_FILE=/etc/aws/config -e AWS_SHARED_CREDENTIALS_FILE=/etc/aws/credentials \
  --mount type=bind,source=/dev,target=/dev \
  --mount type=bind,source=/etc/nova,target=/etc/nova \
  --mount type=bind,source=$HOME/.aws,target=/etc/aws \
  --mount type=bind,source=/sys/devices,target=/sys/devices \
  --mount type=bind,source=/tmp/argus_socket,target=/tmp/argus_socket \
  --mount type=bind,source=/sys/bus/iio/devices,target=/sys/bus/iio/devices \
  --mount type=bind,source=/media/orin_ssd/recordings,target=/media/orin_ssd/recordings \
  --mount type=bind,source=/var/nvidia/nvcam/settings/camera_overrides.isp,target=/var/nvidia/nvcam/settings/camera_overrides.isp \
  nvcr.io/nvstaging/isaac-amr/apps_amr_data_recorder_data_recorder:master-aarch64 --param=uploader.s3_uploader=s3_uploader=bucket=<bucket>
```
I removed the last param in order to get things to run.