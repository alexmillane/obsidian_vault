How to run your python app in GDB:

Add `--config=gdb_python` to you launch command. For example
```bash
dazel run //extensions/nvblox/apps/pod_replay:nvblox_pod_replay_app --config=gdb_python -- --pod-path ~/datasets/2023_04_17_carter_open_loop/2023-04-17_12-32-36_zurich_test_001.pod --publish-mesh
```

These are defined in the `.bazelrc` in the root of the repo as:
```
# Debugging tools

run:gdb_python -c dbg --strip never --run_under "/usr/local/cuda-11.8/bin/cuda-gdb --args python3"

run:gdb -c dbg --strip never --run_under "/usr/local/cuda-11.8/bin/cuda-gdb --args"
```
At the time of writing.