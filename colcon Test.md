
# Testing

```bash
colcon test --packages-select nvblox_ros
colcon test-result --verbose
colcon test-result --delete-yes
```

Raw test output
```bash
cat /workspaces/isaac_ros-dev/build/nvblox_ros/Testing/20221006-1544/Test.xml
```

List of the individual test results in xunit format
```
ls ./build/nvblox_ros/test_results/nvblox_ros/
```
# Linting

```bash
ament_cpplint
```


# nvblox test commands
Run all of our tests
```
`colcon test --packages-up-to-regex '.*nvblox.*'`
```

Run selected tests. In the example below run `nvblox_pol`
```
colcon test --packages-select nvblox_ros --ctest-args -R nvblox_pol --event-handlers console_direct+
```
with pytest
```
colcon test --packages-select nvblox_test --pytest-args -k realsense_example --event-handlers console_direct+
```