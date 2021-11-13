cmake_minimum_required(VERSION 3.15)
project(HelloVulkan)

set(CMAKE_CXX_STANDARD 17)

add_executable(${PROJECT_NAME} Source/main.cpp)

# GLFW
find_package(glfw3 REQUIRED)
target_link_libraries(${PROJECT_NAME} glfw)

# glm
find_package(glm REQUIRED)
target_link_libraries(${PROJECT_NAME} glm::glm)

# Vulkan
find_package(Vulkan REQUIRED)
target_link_libraries(${PROJECT_NAME} Vulkan::Vulkan)
