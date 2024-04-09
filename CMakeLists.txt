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

add_compile_definitions(GLFW_EXPOSE_NATIVE_COCOA)

add_subdirectory(src)
