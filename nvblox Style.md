Let's keep track of important decisions related to style rules and decisions we make.

# Style Guide

In general, we follow [Google Style](https://google.github.io/styleguide/cppguide.html).

# Design goals
A few points:
- **Extensibility:** We template the library on VoxelType. This complicates our lives, but means that the library in generic to the type of data stored on the voxel grid. This was one of the main strengths of voxblox which made it so popular and we've followed that ethos here.
- **No NVCC required in customer code:** We don't want customers to have to compile *their* code with NVCC unless they explicitly are extending nvblox code features. Our public interface should not come with an NVCC dependancy. This means keeping device code out of public header files. In contrast, our code does come with dependancy on `cuda_runtime`.

# Folder Structure

We follow the following folder structure in `nvblox/include`. I'll use the TSDF integrator as an example.
- `nvblox/include/integrators/projectiveTsdfIntegrator.h`
	- This is the public header for this class and functions. This is included via `nvblox.h` by library users.
- `nvblox/include/integrators/internal/projective_integrator.h`
	- Internal contains the definition of classes and functions which are used internally. Customers who want to *extend* nvblox should also expect to use stuff in `internal` folders.
- `nvblox/include/integrators/internal/impl/projective_integrator_impl.h`
	- Implementation of template classes in the preceeding two folders go here.
- `nvblox/include/integrators/internal/cuda/projective_integrators_common.cuh`
	- Definition of template classes/functions containing device code. *Note: because of design goal #2 all CUDA-containing headers **must** be internal, because failure to do so would transmit an NVCC requirement to customers.*
- `nvblox/include/integrators/internal/cuda/impl/projective_integrators_common_impl.cuh`
	- Implementation of template classes/functions, which contain device code, defined in the folder above.

The folder structure of `src` is much simpler:
- `src/integrators/projective_tsdf_integrator.cpp`, `src/integrators/some_other_class.cpp`
	- We put all implementation files (both CUDA and cpp) for all headers in `include/integrators` in a single level folder `src/integrators`. Because implementation files are private there's no need for the strict separation of private and public headers we follow in the include folder.

Rules on `nvblox.h`. For library customers want to *use* nvblox, as opposed to extending nvblox, we private `nvblox.h` which drags in all public headers. We have a lint script to ensure that all new public headers are added to `nvblox.h`.

# Past Decisions
In this section let's write down important style/design decisions as we make them:

-   **What we call an internal header**
	- I take an internal header to mean: A header which should not be used by a library-customer of nvblox who is not extending nvblox core lib. Our ROS wrapper is the prototypical example here. It *uses* nvblox lib, but *does not extend* it by adding new voxel types, integrators etc.

- **CUDA runtime**: Headers containing references to CUDA runtime
	- **Background:** In contrast to code containing CUDA C++ language extensions (kernels, device functions etc), references to cuda runtime do not require compilation with NVCC, only access to the CUDA runtime headers and to be linked against the runtime libraries.
	- **Question:** Do we demarkate headers with references to cuda runtime.
		- **If we do** - Easy separation. Users know which files will imply a dep on cuda_runtime. But making them .cuh will mean they get mixed with NVCC files. Making them yet another extension or in a separate folder will lead to lead to even more proliferatio of folders. And the truth is if you’re using nvblox you need CUDA runtime.
	- **Decision:** 
		- Do not put CUDA runtime dependents in a .cuh file.
		- Put them in a .h file
		- Same folder as other headers.
		- Ramification: Clients should expect a dep on CUDA runtime if they use nvblox.  

- **Classes with part CPU and part GPU code**
	- **Background:** Some (at this point almost all) classes have parts which run on CPU and part on GPU. We *could* separate out the CPU parts into a `.cpp` file to avoid NVCC. This *could* speed up compiliation.
	- **Decision:** Do not do this. This complicates finding the implementation of stuff for little added value.

- **CUDA-related functions which are pure C++ (no NVCC or CUDA runtime)**  
	- **Background:** Some functions are CUDA specific like:
		- `void warmupCuda()`;
		- `float cuda::max(const int rows, const int cols, const float* image);`
	-  do not expose users to device code.
	- **Question:** Do we demarkate these as CUDA-related by placing them in a cuda subfolder?
	- Note: I’m asking because when we first wrote nvblox we did this because we had much more CPU stuff so it made sense to separate what was CPU and GPU this way. I have since eliminated them.
	- **Decision:** No. All files in a `cuda` directory can be expected to contain device code.
    
- **Separation of .cu and .cpp files**
	- **Background:** Early on we separated `.cu` files into `cuda` sub-folder.
	- **Decision:** We eliminated this. No separation required because these aren’t public files. 
	