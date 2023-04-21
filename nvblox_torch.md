
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
