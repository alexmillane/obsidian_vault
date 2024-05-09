
Building a isaac ros debian
```
cd src/isaac_ros_nvblox/nvblox_ros
bloom-generate rosdebian --ros-distro humble
sed -i 's/\(dh_shlibdeps\)/\1 --dpkg-shlibdeps-params=--ignore-missing-info/' debian/rules
sed -i '/override_dh_auto_test:/,/^override_/!b; /override_dh_auto_test:/n; /^override_/b; s/.*/\t@:/' debian/rules
fakeroot debian/rules binary
```
This will generate a debian file one directory up (?!)

## Trialing the debian
Remove the package you would like to overlay
```
sudo dpkg --force-depends --remove ros-humble-nvblox-ros
```
Install the package.
```
sudo dpkg -i ros-humble-nvblox-ros_2.1.1-0jammy_amd64.deb
```
