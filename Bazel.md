


## Linting

Lint things in nvblox
```bash
bazel test //extensions/nvblox/... --config=lint
```

## Buildifier
```bash
bazel run //tools:buildifier_fix_format
```

### Copywrite check
```bash
bazel run //tools/copyright_checker:check_commit
```

Fixes years on touched files
```bash
dazel run //tools/copyright_checker:check_commit -- --fix-year
```

## Clear http archive cache
When updating the nvblox core lib, bazel seemed to refuse to use the new version.

To re-download the 
```bash
bazel sync
```
> Note that this can take a long time


## Cpp Lint
Bazel creates cpp lint targets:
```bash
bazel run //extensions/nvblox/tests:_cpplint_nvblox_pod_replay_lib
```
