set(LIB_NAME tiny_obj_loader)

add_library(${LIB_NAME} tiny_obj_loader.cpp)
target_include_directories(${LIB_NAME} INTERFACE .)
