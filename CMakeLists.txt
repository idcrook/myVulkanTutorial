cmake_minimum_required(VERSION 3.8)

project( myVulkanTutorial )

# message("\nTOP PROJECT = " "${CMAKE_PROJECT_NAME}")
message("PROJECT_NAME = " "${PROJECT_NAME}")

set(CMAKE_MODULE_PATH
  "${CMAKE_CURRENT_SOURCE_DIR}/code/CMake"
  ${CMAKE_MODULE_PATH}
  )

# message("CMAKE_PREFIX_PATH    = " "${CMAKE_PREFIX_PATH}")
# message("CMAKE_MODULE_PATH    = " "${CMAKE_MODULE_PATH}")
# message("CMAKE_BUILD_TYPE     = " "${CMAKE_BUILD_TYPE}")

# Set to c++17 (std::optional, )
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# With multi generators, conan_cmake_run expects a build_type to be provided
if(UNIX AND NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release)
endif()

find_package(Vulkan REQUIRED)
find_package(GLFW   REQUIRED) # ../CMake/FindGLFW.cmake


find_program(Vulkan_GLSLC_EXECUTABLE
      NAMES glslc
      )

# message("Vulkan_GLSLC_EXECUTABLE     = " "${Vulkan_GLSLC_EXECUTABLE}")

# message("Vulkan_INCLUDE_DIR   = " "${Vulkan_INCLUDE_DIR}")
# message("Vulkan_LIBRARY       = " "${Vulkan_LIBRARY}")
# message("GLFW_INCLUDE_DIR     = " "${GLFW_INCLUDE_DIR}")

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
  # code/15_hello_triangle.cpp
  # code/16_swap_chain_recreation.cpp
  # code/17_vertex_input.cpp
  # code/18_vertex_buffer.cpp
  # code/19_staging_buffer.cpp
  # code/20_index_buffer.cpp
  # code/21_descriptor_layout.cpp
  # code/22_descriptor_sets.cpp
  # code/23_texture_image.cpp
  # code/24_sampler.cpp
  # code/25_texture_mapping.cpp
  # code/26_depth_buffering.cpp
  # code/27_model_loading.cpp
  # code/28_mipmapping.cpp
  code/29_multisampling.cpp
  )

# Expect only one shader per category to be active at a time
set (VERTEX_SHADER
  # ${CMAKE_SOURCE_DIR}/shaders/09_shader_base.vert
  # ${CMAKE_SOURCE_DIR}/shaders/17_shader_vertexbuffer.vert
  # ${CMAKE_SOURCE_DIR}/shaders/21_shader_ubo.vert
  # ${CMAKE_SOURCE_DIR}/shaders/25_shader_textures.vert
  ${CMAKE_SOURCE_DIR}/shaders/26_shader_depth.vert
  )

set (FRAGMENT_SHADER
  # ${CMAKE_SOURCE_DIR}/shaders/09_shader_base.frag
  # ${CMAKE_SOURCE_DIR}/shaders/17_shader_vertexbuffer.frag
  # ${CMAKE_SOURCE_DIR}/shaders/21_shader_ubo.frag
  # ${CMAKE_SOURCE_DIR}/shaders/25_shader_textures.frag
  ${CMAKE_SOURCE_DIR}/shaders/26_shader_depth.frag
  )


FUNCTION(add_shader_spv TARGET)
  set(options "")
  set(oneValueArgs OUTPUT_PATH SOURCE SHADER_STAGE)
  set(multiValueArgs DEPENDENCIES)

  CMAKE_PARSE_ARGUMENTS(PARSE_ARGV 1 GLSLC_COMPILE_TO_SPV "${options}" "${oneValueArgs}" "${multiValueArgs}")
  message(STATUS "SOURCE      = " "${GLSLC_COMPILE_TO_SPV_SOURCE}")
  #message(STATUS "OUTPUT_PATH = " "${GLSLC_COMPILE_TO_SPV_OUTPUT_PATH}")

  # assumed to be only one
  FOREACH(input ${GLSLC_COMPILE_TO_SPV_SOURCE})

    # Add a custom command to compile GLSL to SPIR-V.
    add_custom_command(
      OUTPUT  "${GLSLC_COMPILE_TO_SPV_OUTPUT_PATH}"
      COMMAND ${Vulkan_GLSLC_EXECUTABLE} -o "${GLSLC_COMPILE_TO_SPV_OUTPUT_PATH}" "${input}"
      MAIN_DEPENDENCY "${input}"
      WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
      )

    set_source_files_properties(${GLSLC_COMPILE_TO_SPV_OUTPUT_PATH} PROPERTIES GENERATED TRUE)
    target_sources(${TARGET} PRIVATE  "${GLSLC_COMPILE_TO_SPV_OUTPUT_PATH}")
  ENDFOREACH( )
ENDFUNCTION()


add_executable(vulTut
  ${SOURCES}
)

target_include_directories(vulTut PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/3rdparty>
)

add_shader_spv(vulTut
  SOURCE "${VERTEX_SHADER}"
  SHADER_STAGE vert
  OUTPUT_PATH "${CMAKE_SOURCE_DIR}/shaders/vert.spv"
  )

add_shader_spv(vulTut
  SOURCE "${FRAGMENT_SHADER}"
  SHADER_STAGE frag
  OUTPUT_PATH "${CMAKE_SOURCE_DIR}/shaders/frag.spv"
  )


target_link_libraries(vulTut PUBLIC
  Vulkan::Vulkan
  glfw  # v3.3
)
