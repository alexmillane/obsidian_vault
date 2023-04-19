
## Dataloader
Run the replica office0 sequence
```bash
bazel run //extensions/nvblox/apps:nvblox_dataset_replica_office0 -- --param=dataset_loader.dataset_loader=loader=base_path=/home/alex/datasets/replica/office0
```

Run it *and* emit a mesh at the end
```bash
bazel run //extensions/nvblox/apps:nvblox_dataset_replica_office0 -- --param=dataset_loader.dataset_loader=loader=base_path=/home/alex/datasets/replica/office0 --param=nvblox_mapper.nvblox_mapper=nvblox=mesh_ply_path=/home/alex/trunk/nvblox_analysis/2023_03_27_sight_debug/mesh.ply
```


So the general formula for command line parameters is:
```bash
-- --param=subgraph_name.entity_name=component_name=param_name=VALUE
```
