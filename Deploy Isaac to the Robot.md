Build for ARM
`dazel build //extensions/nvblox/apps:nvblox_dataset_replica_office0 --config=jetpack50`

## Deploy 

>Note that you can only deploy `-pkg` targets. Other targets will silently fail in the deploy script.

**Deploy to `localhost` as test:**
```bash
./../engine/engine/build/deploy.sh -b dazel -h localhost -d x86_64_cuda_11_8 -u alex --remote_user alex --run -p //extensions/nvblox/apps:nvblox_dataset_replica_office0-pkg
```

**Deploy to ARM:**

