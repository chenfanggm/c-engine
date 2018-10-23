cmake_minimum_required(VERSION 3.5)

# cmake config
set(CMAKE_BUILD_TYPE Debug)
set(CMAKE_VERBOSE_MAKEFILE ON)
set(CMAKE_CXX_COMPILER "/usr/bin/clang++")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++17 -stdlib=libc++")

# project config
project(vs-cpp)
set(SOURCE_DIR "${PROJECT_SOURCE_DIR}/src/")
file(GLOB SOURCE_FILES "${SOURCE_DIR}/*.cpp")

# target config
add_executable(${PROJECT_NAME} ${SOURCE_FILES})
set_target_properties(${PROJECT_NAME} 
    PROPERTIES 
    COMPILE_FLAGS "-save-temps"
)