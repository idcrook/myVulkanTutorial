cmake_minimum_required(VERSION 3.8)

project( myVulkanTutorial )

message("\nTOP PROJECT = " "${CMAKE_PROJECT_NAME}")
message("PROJECT_NAME = " "${PROJECT_NAME}")

set(CMAKE_MODULE_PATH
  "${CMAKE_CURRENT_SOURCE_DIR}/code/CMake"
  ${CMAKE_MODULE_PATH}
  )

message("CMAKE_PREFIX_PATH    = " "${CMAKE_PREFIX_PATH}")
message("CMAKE_MODULE_PATH    = " "${CMAKE_MODULE_PATH}")
message("CMAKE_BUILD_TYPE     = " "${CMAKE_BUILD_TYPE}")

# Set to c++17 (std::optional, )
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# With multi generators, conan_cmake_run expects a build_type to be provided
if(UNIX AND NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release)
endif()

find_package(Vulkan REQUIRED)
find_package(GLFW   REQUIRED) # ../CMake/FindGLFW.cmake

set( HEADERS
  )

set( SOURCES
  # code/00_base_code.cpp
  # code/01_instance_creation.cpp
  # code/02_validation_layers.cpp
  # code/03_physical_device_selection.cpp
  # code/04_logical_device.cpp
  # code/05_window_surface.cpp
  # code/06_swap_chain_creation.cpp
  # code/07_image_views.cpp
  # code/08_graphics_pipeline.cpp
  # code/09_shader_modules.cpp
  # code/10_fixed_functions.cpp
  # code/11_render_passes.cpp
  # code/12_graphics_pipeline_complete.cpp
  # code/13_framebuffers.cpp
  # code/14_command_buffers.cpp
  code/15_hello_triangle.cpp
  # code/16_swap_chain_recreation.cpp
  # code/17_vertex_input.cpp
  # code/18_vertex_buffer.cpp
  # code/19_staging_buffer.cpp
  # code/20_index_buffer.cpp
  # code/21_descriptor_layout.cpp
  # code/21_shader_ubo.frag
  # code/21_shader_ubo.vert
  # code/22_descriptor_sets.cpp
  # code/23_texture_image.cpp
  # code/24_sampler.cpp
  # code/25_texture_mapping.cpp
  # code/26_depth_buffering.cpp
  # code/27_model_loading.cpp
  # code/28_mipmapping.cpp
  # code/29_multisampling.cpp
  )

# FIXME: incorporate into cmake the shader files and compiling them
  # code/09_shader_base.frag
  # code/09_shader_base.vert
  # code/17_shader_vertexbuffer.frag
  # code/17_shader_vertexbuffer.vert
  # code/26_shader_depth.frag
  # code/26_shader_depth.vert
  # code/25_shader_textures.frag
  # code/25_shader_textures.vert

message("Vulkan_INCLUDE_DIR   = " "${Vulkan_INCLUDE_DIR}")
message("GLFW_INCLUDE_DIR     = " "${GLFW_INCLUDE_DIR}")

add_executable(vulTut
  ${HEADERS}
  ${SOURCES}
)

target_link_libraries(vulTut PUBLIC
  Vulkan::Vulkan
  glfw  # v3.3
)
