vulkan tutorial
===============

based on https://vulkan-tutorial.com/

pre-reqs
--------

1.	Vulkan SDK (lib and headers) installed
	-	For me this was installing Vulkan 1.2 Developer Beta Driver
	-	From https://developer.nvidia.com/vulkan-driver
2.	glfw3 lib and headers installed
3.	GLM lib and headers installed
4.	build tool CMake installed

on ubuntu eoan (19.10), for things other than Vulkan SDK:

```
sudo apt install cmake
sudo apt install libglfw3-dev
sudo apt install libglm-dev
```

Build (cmake)
-------------

```shell
# generate
cmake \
    -D CMAKE_BUILD_TYPE=Release \
    -B build .

# build
VERBOSE=1 cmake --build build --target vulTut --parallel 7 --clean-first
# (inspect CMakeLists.txt for what will be built)

# run
build/vulTut
```
