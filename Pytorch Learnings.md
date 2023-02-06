# Installation
I decided to go with an container based approach from NGC. *Maybe* this the most NVIDIA-y? I found the newest version of the pytorch container for my CUDA version on my laptop from here: https://docs.nvidia.com/deeplearning/frameworks/pytorch-release-notes/rel-22-12.html#rel-22-12.

# vscode Setup
So to develop using pytorch inside the container, as to use vscode features like autocompletion, I need vscode running *inside* the container where it has access to the pytorch installation. For that reason I completed/installed the dev container stuff here: https://code.visualstudio.com/docs/devcontainers/tutorial

> OK so the dev container guide recommended Docker Desktop. This seemed to not interact well with the nvidia-container runtime. It appears that Docker Desktop install a whole new docker install, separate from the one I already had install. It turns out if I turn off docker desktop, everything works.

# Notes for Next Time
Notes:
- I still don't have autocomplete working. For now I'm giving up.
- Going back to refactoring.