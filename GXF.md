## Build file
data = graph + config deps
extensions = library/binary deps

## Interfaces
Interfaces to a subgraph define signals from the subgraph which appear in the parent scope. Apparently internal signals also appear but are namespaced.

# Errors


## "Push failed on rx_tensor"
```
2023-02-15 10:25:17.151 WARN external/com_nvidia_gxf/gxf/std/double_buffer_receiver.cpp@78: Push failed on 'rx_tensor'
Failed to sync outbox for entity: depth_broadcaster.broadcast code: GXF_EXCEEDING_PREALLOCATED_SIZE
```
The problem here is that the input channel is being blow. The solution is to set `policy 0` on the receiver buffer.

For example here:
```
name: depth_visualizer
components:
- name: rx_tensor
  type: nvidia::gxf::DoubleBufferReceiver
  parameters:
    policy: 0
```


## Prerequisites
Must be components

## Unique IDs
Use Clemens' alias
```bash
generateHex
```
which translates to
```bash
alias generateHex='printf "0x%s\n" $(uuidgen -r | tr -d - | cut -b 1-16)'
```

## Override
Name + type = creates new component
name = adds to component

## Run with nsight

```bash
bazel run //extensions/nvblox/apps/pod_replay:nvblox_pod_replay -- --profile --export
```

## Compile Commands (autocomplete)
```bash
bazel run //tools:generate_compile_commands
```


# Linting
Lint everything:
```bash
bazel test --config=lint -- ... -//packages/...
```

Lint a folder
```bash
bazel test //extensions/nvblox/... --config=lint
```

Lint a single target
```bash
bazel run //extensions/vslam/apps:_gxflint_<YOUR_TARGET_NAME>
```

For example
```bash
bazel run //extensions/vslam/apps:_gxflint_rs_tracker
```

