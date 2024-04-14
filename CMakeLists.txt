cmake_minimum_required(VERSION 3.15)
project(WgpuNativeMac)

set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED ON)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

include(cmake/CPM.cmake)
CPMAddPackage(
  NAME glfw
  GITHUB_REPOSITORY glfw/glfw
  GIT_TAG 3.4
  VERSION 3.4
  OPTIONS
    "BUILD_SHARED_LIBS OFF"
    "GLFW_BUILD_EXAMPLES OFF"
    "GLFW_BUILD_TESTS OFF"
    "GLFW_BUILD_DOCS OFF"
    "GLFW_INSTALL OFF"
)
CPMAddPackage(
  NAME wgpu-native
  URL https://github.com/gfx-rs/wgpu-native/releases/download/v0.19.3.1/wgpu-macos-aarch64-release.zip
  VERSION 0.19.3.1
  DOWNLOAD_ONLY True
)
CPMAddPackage(
  NAME glfw3webgpu
  GITHUB_REPOSITORY eliemichel/glfw3webgpu
  GIT_TAG v1.2.0
  VERSION 1.2.0
  DOWNLOAD_ONLY True
)
if(glfw3webgpu_ADDED)
  if(NOT EXISTS "${wgpu-native_SOURCE_DIR}/webgpu/webgpu.h")
    file(MAKE_DIRECTORY "${wgpu-native_SOURCE_DIR}/webgpu")
    file(COPY "${wgpu-native_SOURCE_DIR}/webgpu.h" DESTINATION "${wgpu-native_SOURCE_DIR}/webgpu/")
  endif()

  add_library(glfw3webgpu ${glfw3webgpu_SOURCE_DIR}/glfw3webgpu.c)
  target_include_directories(glfw3webgpu
    PUBLIC "${glfw3webgpu_SOURCE_DIR}"
           "${wgpu-native_SOURCE_DIR}")
  target_link_directories(glfw3webgpu
    PUBLIC "${wgpu-native_SOURCE_DIR}")
  target_link_libraries(glfw3webgpu PUBLIC glfw wgpu_native)

  if (APPLE)
    target_compile_options(glfw3webgpu PRIVATE -x objective-c)
    target_link_libraries(glfw3webgpu PRIVATE "-framework Cocoa" "-framework CoreVideo" "-framework IOKit" "-framework QuartzCore")
  endif ()
endif()

add_subdirectory(src)
