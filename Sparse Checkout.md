We had to checkout a single submodule of a repo for Jenkins to get the right version of `realsense_ros` from isaac_ros

```bash
cd workspaces/isaac_ros-dev/src
mkdir isaac_ros_dev
cd isaac_ros_dev
git init
git remote add -f origin ssh://git@gitlab-master.nvidia.com:12051/isaac_ros/isaac_ros-dev.git
git config core.sparseCheckout true
echo "ros_ws/src/third_party/realsense-ros" >> .git/info/sparse-checkout
git pull origin main
git submodule update --init ros_ws/src/third_party/realsense-ros/
```
