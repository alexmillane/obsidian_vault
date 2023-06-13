
# Currently working setup
At the moment I have a docker file based on the NVIDIA pytorch docker. One you have things built, to run nvblox_torch (see build instructions below):

Can run this docker with
```bash
cd ~/trunk/nvblox_torch
./run_docker.sh
```
Then install nvblox_torch
```bash
cd nvblox_torch
pip install -e .
```
Then run the example
```bash
cd examples
python run_mapper.py --dataset /home/alex/datasets/3d_match/sun3d-mit_76_studyroom-76-1studyroom2 --dataset-format sun3d --voxel-size 0.05
```

## Build process
I have modified the build file such that to build you do:
```bash
cd ~/trunk/nvblox_torch
./run_docker.sh
cd nvblox_torch
./install.sh
```

# Installation process
Things I've had to change

- Add variables for the paths to `nvblox` and `libtorch` installation directories. The paths point to the install folders
- Modify which nvblox target was being linking against to `nvblox::nvblox_lib`
	- This mean that the `find_package(Threads)` issue came up again and I had to add that too.
	- I should probably sort that threads thing out.

# Using nvblox_torch in VScode

I wanna use nvblox torch inside the vscode interactive window. To do so

- In the terminal activate conda `conda activate`.
- Then open vscode with `code-insiders`
- It will prompt you to switch python version to the anaconda one
- You may also have to switch the kernel



## Trying to build from source

### CuDNN
- Downloaded the local debian
- ran `sudo apt-get install libcudnn8-dev=8.9.2.26-1+cuda11.8`

> Building from source worked, but took ages. Trying to move to a docker-based build


## Docker Build
Trying a version of the pytorch docker which runs with CUDA 11.8 and NVIDIA driver 520
```bash
docker run --gpus all -it --rm nvcr.io/nvidia/pytorch:22.12-py3
```
I now added nvblox as a submodule such that we can work with a local version. This makes the run command
```

```

## Useful

```
torch.cuda.is_available()
```
```
torch.compiled_with_cxx11_abi()
```