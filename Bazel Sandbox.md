

# Rough Notes
Clean these up later.

## Sandbox location
The sandbox is located in `isaac/sdk/bazel-bin`

## Looking for shitty extension .so files
So each app has its own space in the sandbox, for example for the `nvblox_pod_replay_python` app that I'm working on it's at:
```bash
{SANDBOX}/extensions/nvblox/apps/pod_replay/nvblox_pod_replay_python
```
Then each of the deps has its own folder below that. For example for the decoder extension
```bash
{SANDBOX}/extensions/nvblox/apps/pod_replay/nvblox_pod_replay_python.runfiles/com_nvidia_isaac_sdk/extensions/codec
```

