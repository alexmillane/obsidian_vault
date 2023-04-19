
# Testing

```bash
colcon test --packages-select nvblox_nav2
colcon test-result --verbose
colcon test-result --delete-yes
```

Raw test output
```bash
cat /workspaces/isaac_ros-dev/build/nvblox_ros/Testing/20221006-1544/Test.xml
```

# Linting

```bash
ament_cpplint
```
