cmake_minimum_required(VERSION 3.5)
project(vs-cpp)

# print evironment
if(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
	message("============= Mac OS ==============")
elseif(${CMAKE_SYSTEM_NAME} MATCHES "Linux")
	message("============= Linux ==============")
elseif(${CMAKE_SYSTEM_NAME} MATCHES "Windows")
	message("============= Windows ==============")
endif(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")

message("CMAKE_SOURCE_DIR = ${CMAKE_SOURCE_DIR}")
message("CMAKE_MODULE_PATH = ${CMAKE_MODULE_PATH}") # default empty
message("CMAKE_PREFIX_PATH = ${CMAKE_PREFIX_PATH}") # default empty

# cmake config
set(CMAKE_BUILD_TYPE Debug)
set(CMAKE_VERBOSE_MAKEFILE ON)
set(CMAKE_CXX_COMPILER "/usr/bin/clang++")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++17 -stdlib=libc++")

# dependencies
find_package(OpenGL REQUIRED) # OPENGL_INCLUDE_DIR, OPENGL_LIBRARIES
find_package(glfw3 3.2 REQUIRED)
find_package(GLEW REQUIRED STATIC)
find_package(GLM REQUIRED)

message("============== Includes ==============")
message("OPENGL_INCLUDE_DIR=${OPENGL_INCLUDE_DIR}")
message("GLFW_INCLUDE_DIR=${GLFW_INCLUDE_DIR}")
message("GLEW_INCLUDE_DIR=${GLEW_INCLUDE_DIR}")
message("GLM_INCLUDE_DIR=${GLM_INCLUDE_DIR}")
message("============== Libaries ==============")
message("OPENGL_LIBRARIES=${OPENGL_LIBRARIES}")
message("GLFW_LIBRARIES=${GLFW_LIBRARIES}")
message("GLEW_LIBRARIES=${GLEW_LIBRARIES}")
message("GLM_LIBRARIES=${GLM_LIBRARIES}")

if (APPLE)
find_library(COCOA_LIBRARY Cocoa REQUIRED)
find_library(IOKIT_LIBRARY IOKit REQUIRED)
find_library(COREVID_LIBRARY CoreVideo REQUIRED)
endif (APPLE)

# source
set(SOURCE_DIR "${PROJECT_SOURCE_DIR}/src/")
file(GLOB SOURCE_FILES "${SOURCE_DIR}/*.cpp")
file(GLOB SOURCE_HEADER_FILES "${SOURCE_DIR}/*.hpp")

# executable
add_executable(${PROJECT_NAME} ${SOURCE_HEADER_FILES} ${SOURCE_FILES})
set_target_properties(${PROJECT_NAME} 
    PROPERTIES 
    COMPILE_FLAGS "-save-temps"
)

# link
target_include_directories(${PROJECT_NAME} PRIVATE 
    ${OPENGL_INCLUDE_DIR}
    ${GLFW_INCLUDE_DIR}
    ${GLEW_INCLUDE_DIR}
    ${GLM_INCLUDE_DIR}
)
target_link_libraries(${PROJECT_NAME} 
    ${OPENGL_LIBRARIES}
    glfw
    ${GLEW_LIBRARIES}
    ${GLM_LIBRARIES}
    ${COCOA_LIBRARIES} ${COREVID_LIBRARIES} ${IOKIT_LIBRARIES} # mac only
)
