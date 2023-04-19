Had to do this to trigger a run of the isaac_ros pipeline.

Query open pull requests on isaac_ros_nvblox
```bash
curl --header "PRIVATE-TOKEN: glpat-tfg8TK4GTFGmokNma7zc" "https://gitlab-master.nvidia.com/api/v4/projects/56804/merge_requests?state=opened"
```
Where the access token I generated for the occasion

I then searched the return of the relavent strings and triggered the isaac ros pipeline with te string:
```
{ "merge_request_to_report_to": [ { "head_commit_id": "f12a456800729da873a09dcdf19ebb2cf35547e0", "merge_request_id": 1276983, "merge_request_iid": 314, "project_id": 56804 } ], "submodules_checkout": [ { "submodule_ref": "f12a456800729da873a09dcdf19ebb2cf35547e0", "submodule_dir": "ros_ws/src/isaac_ros_nvblox" } ] }
```

## Gitlab access token
glpat-tfg8TK4GTFGmokNma7zcz

