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

# stb_image
add_subdirectory(External/stb_image)
target_link_libraries(${PROJECT_NAME} stb_image)

# ============================================ Assets ============================================

# Compiled shaders and move the outputs to destination directories
set(SHADER_SOURCE_DIR ${PROJECT_SOURCE_DIR}/Shaders)
set(SHADER_BINARY_DIR ${PROJECT_BINARY_DIR}/Shaders)

file(MAKE_DIRECTORY ${SHADER_BINARY_DIR})

file(GLOB SHADERS
        ${SHADER_SOURCE_DIR}/*.vert
        ${SHADER_SOURCE_DIR}/*.frag
        )

foreach(source IN LISTS SHADERS)
    get_filename_component(filename ${source} NAME)
    add_custom_command(
            COMMAND glslc -o ${SHADER_BINARY_DIR}/${filename}.spv ${source}
            OUTPUT ${SHADER_BINARY_DIR}/${filename}.spv
            DEPENDS ${source} ${SHADER_SOURCE_DIR}
    )
    list(APPEND SPV_SHADERS ${SHADER_BINARY_DIR}/${filename}.spv)
endforeach()

add_custom_target(shaders DEPENDS ${SPV_SHADERS})

add_dependencies(${PROJECT_NAME} shaders)

# Copy textures to destination directories
set(TEXTURE_SOURCE_DIR ${PROJECT_SOURCE_DIR}/Textures)
set(TEXTURE_DST_PARENT_DIR ${PROJECT_BINARY_DIR})

file(COPY ${TEXTURE_SOURCE_DIR} DESTINATION ${TEXTURE_DST_PARENT_DIR})
