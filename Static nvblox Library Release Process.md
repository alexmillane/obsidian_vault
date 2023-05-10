

# Build the Redistributable Library
This assumes tou have the dependencies compiled as described below. I put everything in a folder called `nvblox_`

Assuming you have all the dependancies below already setup do the following.

Navigate:
`cd /home/alex/trunk/standalone_libraries/nvblox/nvblox/build`

Build:

```bash
cmake .. -DCMAKE_INSTALL_PREFIX=/home/ibex/trunk/nvblox_static_lib/nvblox/nvblox/install/ -DBUILD_FOR_ALL_ARCHS=TRUE -DBUILD_REDISTRIBUTABLE=TRUE -DSQLITE3_BASE_PATH="/home/ibex/trunk/nvblox_static_lib/sqlite-autoconf-3390400/install/" -DGLOG_BASE_PATH="/home/ibex/trunk/nvblox_static_lib/glog-0.4.0/install/" -DGFLAGS_BASE_PATH="/home/ibex/trunk/nvblox_static_lib/gflags-2.2.2/install/" && make -j8 && make install
```

# Distribute via artifactory 
We put the shared libraries for both `x86_64` and `arm64` into a zip archive and then upload it to artifactory.

## Create the Artifact
I make a new folder called `artifact`. Inside it I drag the headers from x86 install folder (`nvblox/nvblox/install/include`) and the x86 libs and arm64 libs (`nvblox/nvblox/install/lib`), and rename their folders such that the overall folder structure looks like:
```bash
/artifact
  /include
  /lib_x86_64
  /lib_aarch64
```

Zip up the subfolders *inside* of the artifact directory. Rename the zip file to reflect the version number. This time I did `nvblox_alpha6.zip`.

> **Note:** make sure that you zip the folders *inside* the version folder. Double clicking on the zip folder should show you the lib folders in the first level. i.e. `lib_x86_64` should be viewable immediately inside.

## Upload the Artifact
Then, upload the zip file to artifactory by:

1) Navigate to the `sw-isaac-sdk-generic-local` artifacts by going to the URL:
```
*https://urm.nvidia.com/ui/repos/tree/General/sw-isaac-sdk-generic-local*
```
2) Upload the asset by clicking the "Deploy" button and then dragging the zip folder into the box.
3) Wait for upload

> After upload you'll be prompted with another prompt. Don't click either of the two "deploy as" boxes.

4) Fill our the "Target Path" to prepend the internal deps location. For my example this box would contain: `/dependencies/internal/nvblox_alpha6.zip`

5) Navigate to the artifact in the toolbar on the left and grab the SHA256 hash. You'll need this for your bazel build file.

# Update in Bazel
Update the zip file name and SHA256 in: `isaac/sdk/extensions/third_party/packages.bzl`


# Build the Dependencies

## SQLITE3
Get it from here: https://sqlite.org/download.html
```bash
wget https://sqlite.org/2022/sqlite-autoconf-3400100.tar.gz
tar -xvf sqlite-autoconf-3400100.tar.gz
cd sqlite-autoconf-3400100
mkdir install
CFLAGS=-fPIC ./configure --prefix=/home/ibex/trunk/nvblox_static_lib/sqlite-autoconf-3400100/install/
make
```

Path:
`/home/alex/trunk/standalone_libraries/sqlite-autoconf-3390400`


## GLOG
Note for some reason when we originally did this we used version 0.4.0... I guess maybe this was to align with the version we get off apt on x86. But I'm not sure. In any case, I am just going to continue with 0.4.0.

Get if from here:
```bash
wget https://github.com/google/glog/archive/refs/tags/v0.4.0.tar.gz
tar -xvf v0.4.0.tar.gz
cd glog-0.4.0
mkdir build
mkdir install
cd build
cmake .. -DCMAKE_POSITION_INDEPENDENT_CODE=ON -DCMAKE_INSTALL_PREFIX=/home/alex/trunk/nvblox_static_lib/glog-0.4.0/install/ -DWITH_GFLAGS=OFF -DBUILD_SHARED_LIBS=OFF
make -j8
make install
```

## GFLAGS
Get it from the releases tab on 
```bash
wget https://github.com/gflags/gflags/archive/refs/tags/v2.2.2.tar.gz
tar -xvf v2.2.2.tar.gz
cd gflags-2.2.2/
mkdir build
mkdir install
cd build
cmake .. -DCMAKE_POSITION_INDEPENDENT_CODE=ON -DCMAKE_INSTALL_PREFIX=/home/alex/trunk/nvblox_static_lib/gflags-2.2.2/install/ -DGFLAGS_BUILD_STATIC_LIBS=ON -DGFLAGS=google && make -j8 && make install
```
  
  

# Test New Library Before Deployment

### nvblox lib side
Check out your development branch in the `~/trunk/nvblox_static_lib`
```bash
cd ~/trunk/nvblox_static_lib/nvblox/scripts
./make_deployable_artifact
```
Copy the results it to the staging area
```bash
cp -r ../nvblox/install/lib/ ../../artifact_for_deployment/testing/lib_x86_64
cp -r ../nvblox/install/include/ ../../artifact_for_deployment/testing/include
```

### Mono-repo side
Replace the nvblox target with a local resource:
```python
isaac_new_local_repository(
	name = "nvblox",
	build_file = clean_dep("//third_party:nvblox.BUILD"),
	path = "/home/alex/trunk/nvblox_static_lib/artifact_for_deployment/testing",
	# sha256 = "9062f4629048185f8c70588fb6d2f7e6d45ec9e0bf1868c2c39b4b34689c3722",
	licenses = ["https://github.com/nvidia-isaac/nvblox/blob/public/LICENSE"],
)
```
Unfortunately bazel can't detect that something has changed so you have to force it to do a sync 

You can force an update by adding a wrong SHA. This *may* force an update. Needs testing.
```bash

```
>Not sure if the above statements are actually true with the local repo. **Update next time you check this!!!**