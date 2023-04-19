
How to get your app image on NGC. Taken from [OSMO guide](https://docs.google.com/document/d/1CWPEh3JP80KAjp2jyx1D--2UtbdsPpFGHx2vjiQ7qn0/edit#heading=h.ynejciqb5qie)

build
```bash
bazel build //extensions/nvblox/apps/datasets:nvblox_dataset_replica_office0-image
```

test
```bash
bazel run //extensions/nvblox/apps/datasets:nvblox_dataset_replica_office0-image
```

push
```bash
bazel run //extensions/nvblox/apps/datasets:nvblox_dataset_replica_office0-push
```
