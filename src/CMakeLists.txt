add_library(framework framework.c)
target_link_libraries(framework PUBLIC glfw3webgpu)

add_executable(main main.c)
target_link_libraries(main PUBLIC framework)
