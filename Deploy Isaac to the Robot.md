Build for ARM
`dazel build //extensions/nvblox/apps:nvblox_dataset_replica_office0 --config=jetpack50`

## Deploy 

>Note that you can only deploy `-pkg` targets. Other targets will silently fail in the deploy script.

**Deploy to `localhost` as test:**
```bash
./../engine/engine/build/deploy.sh -b dazel -h localhost -d x86_64_cuda_11_8 -u alex --remote_user alex --run -p //extensions/nvblox/apps:nvblox_dataset_replica_office0-pkg
```

**Deploy to ARM:**


# Common deploy

Open Loop
```bash
./../engine/engine/build/deploy.sh -h carter-v23-9.client.nvidia.com -d jetpack51 -p //extensions/nvblox/apps/carter_open_loop:carter_open_loop-pkg
```

Closed Loop
```bash
./../engine/engine/build/deploy.sh -h carter-v23-9.client.nvidia.com -d jetpack51 -p //apps/amr/navigation_stack:carter_demo_occupancy_nvblox_environment_zurich-pkg
```
