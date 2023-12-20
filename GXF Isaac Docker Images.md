
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

> **NOTE:** This last target, the push target, needs to be run in bazel, not just built.


# Python
Notes I made while getting the python images going

build is the same command
```bash
bazel build //extensions/nvblox/apps/pod_replay:nvblox_pod_replay-image
```

test
```bash
bazel build //extensions/nvblox/apps/pod_replay:nvblox_pod_replay-image
```
> Note: I don't know how to pass arguments to docker when running this, so the app seems to start, but then fails because the dataset path is not mapped.

push
```bash
bazel build //extensions/nvblox/apps/pod_replay:nvblox_pod_replay-push
```
> NOTE: The tag you get is you 


## Bazel image target
Add the fields `nvcr_container`, `nvcr_org`, `docker_base_image` such that we get for the `nvblox_pod_replay` app, for example, we get:
```python
isaac_gxf_app(
	name = "nvblox_pod_replay",
	srcs = ["nvblox_pod_replay.yaml"],
	data = DATA,
	extensions = EXTENSIONS,
	nvcr_container = "nvblox_pod_replay",
	nvcr_org = "nvstaging/isaac-amr",
	docker_base_image = "//third_party/internal:docker_base_image",
	tags = ["sqa"],
)
```
In the `isaac_gxf_app` rule it says not to specify the container name using `nvcr_container` but from looking around (briefly) it looks like people are...