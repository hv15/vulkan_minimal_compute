cmake_minimum_required (VERSION 3.8)
project (vulkan_minimal_compute)

find_package(PkgConfig)

pkg_check_modules(Vulkan REQUIRED vulkan>=1.1.128)

# get rid of annoying MSVC warnings.
add_definitions(-D_CRT_SECURE_NO_WARNINGS)

set (CMAKE_CXX_STANDARD 11)

include_directories(${Vulkan_INCLUDE_DIR})

add_custom_target (shader_comp DEPENDS comp.spv)

add_custom_command (OUTPUT comp.spv
    COMMAND /usr/bin/glslangValidator -V ${CMAKE_SOURCE_DIR}/shaders/shader.comp
    MAIN_DEPENDENCY ${CMAKE_SOURCE_DIR}/shaders/shader.comp)

add_executable(vulkan_minimal_compute src/main.cpp src/lodepng.cpp)

set_target_properties(vulkan_minimal_compute PROPERTIES VS_DEBUGGER_WORKING_DIRECTORY "${CMAKE_SOURCE_DIR}")

target_link_libraries(vulkan_minimal_compute ${Vulkan_LIBRARIES} ${CMAKE_DL_LIBS})

add_dependencies (vulkan_minimal_compute shader_comp)
