add_library(framework framework.c)
target_include_directories(framework PUBLIC "${wgpu-native_SOURCE_DIR}")
target_link_directories(framework PUBLIC "${wgpu-native_SOURCE_DIR}")
target_link_libraries(framework PUBLIC wgpu_native glfw "-framework Cocoa" "-framework Metal" "-framework QuartzCore")

add_executable(main main.m)
target_link_libraries(main PUBLIC framework)
