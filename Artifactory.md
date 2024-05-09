

## Uploading via the CLI

Compress a folder into a dataset
```
tar -czvf 2024_01_31_with_obstacles_no_lidar_cut_30s.tar.gz 2024_01_31_with_obstacles_no_lidar_cut_30s
```

To upload a dataset:
```
jfrog rt upload 2024_01_31_with_obstacles_no_lidar_30s.tar.gz sw-isaac-sdk-generic-local/dependencies/internal/data/
```

## Configure the cli

To check your config:  `jfrog c s`

To add a new congif: `jfrog c a`

My current config file (`cat ~/.jfrog/jfrog-cli.conf.v5`)

CANT PUT THE CONTENTS HERE BECAUSE GITHUB PREVENTS PUSHING ACCESS TOKENS.

