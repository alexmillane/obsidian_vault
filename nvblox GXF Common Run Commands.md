

# POD replay
Different camera commands

ESS
```bash
dazel run //extensions/nvblox/apps/pod_replay:nvblox_pod_replay -- --pod-path ~/datasets/2023_04_17_carter_open_loop/2023-04-17_13-56-10_zurich_test_003.pod --camera hawk --depth ess
```


# Tests
Run our tests commands

## nvblox POD file tests
We define tests for each camera/stereo type.

realsense
```bash
dazel test //extensions/nvblox/tests:test_nvblox_pod_replay_realsense
```

ESS
```bash
dazel test //extensions/nvblox/tests:test_nvblox_pod_replayess
```

SGM
```bash
dazel test //extensions/nvblox/tests:test_nvblox_pod_replay_sgm
```

# Deploy
Deploy shit to the robot

## Carter open loop
```bash
./../engine/engine/build/deploy.py -r carter-v23-9.dyn.nvidia.com -c jetpack51 -t //extensions/nvblox/apps/carter_open_loop:nvblox_carter_open_loop
```

## Navigation Stack
```bash
./../engine/engine/build/deploy.py -r carter-v23-9.dyn.nvidia.com -c jetpack51 -t //extensions/navigation_stack/apps/navigation_stack:navigation_stack
```

# Benchmark

