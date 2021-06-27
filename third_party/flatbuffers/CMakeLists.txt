cmake_minimum_required(VERSION 3.15)
project(flatbuffers_3rdparty)

include(ExternalProject)
include(VisualStudio)

set(EXT_SOURCE_DIR "" CACHE PATH "${CMAKE_BINARY_DIR}/source")
set(EXT_BUILD_DIR "" CACHE PATH "${CMAKE_BINARY_DIR}/build")
set(EXT_INSTALL_DIR "" CACHE PATH "${CMAKE_BINARY_DIR}/install")

file(WRITE "${CMAKE_CURRENT_BINARY_DIR}/cmake_command.bat" "call \"${VS_DEV_CMD}\" -arch=amd64 & \"${CMAKE_COMMAND}\" %*")

get_filename_component(C_COMPILER ${CMAKE_C_COMPILER} NAME)
get_filename_component(CXX_COMPILER ${CMAKE_CXX_COMPILER} NAME)

ExternalProject_Add(flatbuffers_3rdparty
    GIT_REPOSITORY https://github.com/google/flatbuffers
    GIT_TAG v1.12.0
    SOURCE_DIR ${EXT_SOURCE_DIR}
    BINARY_DIR ${EXT_BUILD_DIR}
    CMAKE_ARGS
        -DCMAKE_C_COMPILER=${C_COMPILER}
        -DCMAKE_CXX_COMPILER=${CXX_COMPILER}
        -G${CMAKE_GENERATOR}
        -DCMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE}
        "-DCMAKE_INSTALL_PREFIX=${EXT_INSTALL_DIR}"
        "-DCMAKE_PREFIX_PATH=${EXT_INSTALL_DIR}"
        -DCMAKE_C_FLAGS=-w
        -DCMAKE_CXX_FLAGS=-w
        -DBUILD_SHARED_LIBS=${BUILD_SHARED_LIBS}
    CMAKE_GENERATOR ${CMAKE_GENERATOR}
    CMAKE_COMMAND cmd /C "${CMAKE_CURRENT_BINARY_DIR}/cmake_command.bat" 
)