Hostname
```bash
carter-v23-9.client.nvidia.com
```
password:
```bash
$uperNOVA23
```

to connect use my alias
```bash
marmot
```

## Run the joystick
Info taken from [Isaac Dev Guide](https://docs.google.com/document/d/1YwSaGEUd_0eQCRvETEcHHGZgYmrM62U9CyZa-Y_hhzw/edit#) and the [Isaac 2.0 dox](https://isaac.nvidia.com/master/extensions/navigation_stack/doc/03_navigation_stack_on_carter.html).

Build the joystick app for segway
```bash
bazel build //apps/amr/robot_remote_control:robot_remote_control_segway
```
Deploy the joystick to marmot
```bash
./../engine/engine/build/deploy.sh -h carter-v23-9.client.nvidia.com -d jetpack50 -p //apps/amr/robot_remote_control:robot_remote_control_segway-pkg
```
Run the joystick on marmot
```bash
# Log on 
marmot
# Run that bitch
cd ~/deploy/lgulich/robot_remote_control_segway-pkg
./apps/amr/robot_remote_control/robot_remote_control_segway
```


## Run the open-loop nvblox app
To deply the open loop app
```bash
./../engine/engine/build/deploy.sh -h carter-v23-9.client.nvidia.com -d jetpack50 -p //extensions/nvblox/apps/carter_open_loop:carter_open_loop-pkg
```

## Record some fucking data bitches
[[GXF Data Recorder]]
