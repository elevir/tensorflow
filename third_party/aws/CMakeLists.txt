cmake_minimum_required(VERSION 3.15)
project(aws_3rdparty)

include(ExternalProject)
include(VisualStudio)

set(EXT_SOURCE_DIR "" CACHE PATH "${CMAKE_BINARY_DIR}/source")
set(EXT_BUILD_DIR "" CACHE PATH "${CMAKE_BINARY_DIR}/build")
set(EXT_INSTALL_DIR "" CACHE PATH "${CMAKE_BINARY_DIR}/install")

file(WRITE "${CMAKE_CURRENT_BINARY_DIR}/cmake_command.bat" "call \"${VS_DEV_CMD}\" -arch=amd64 & \"${CMAKE_COMMAND}\" %*")

get_filename_component(C_COMPILER ${CMAKE_C_COMPILER} NAME)
get_filename_component(CXX_COMPILER ${CMAKE_CXX_COMPILER} NAME)

ExternalProject_Add(aws_sdk_cpp_3rdparty
    GIT_REPOSITORY https://github.com/aws/aws-sdk-cpp
    GIT_TAG 1.7.336
    SOURCE_DIR ${EXT_SOURCE_DIR}/aws-sdk-cpp
    BINARY_DIR ${EXT_BUILD_DIR}/aws-sdk-cpp
    CMAKE_ARGS
        -DCMAKE_C_COMPILER=${C_COMPILER}
        -DCMAKE_CXX_COMPILER=${CXX_COMPILER}
        -G${CMAKE_GENERATOR}
        -DCMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE}
        "-DCMAKE_INSTALL_PREFIX=${EXT_INSTALL_DIR}"
        "-DCMAKE_PREFIX_PATH=${EXT_INSTALL_DIR}"
        "-DCMAKE_C_FLAGS=-w -fexceptions"
        "-DCMAKE_CXX_FLAGS=-w -fcxx-exceptions"
        -DBUILD_SHARED_LIBS=${BUILD_SHARED_LIBS}
    CMAKE_GENERATOR ${CMAKE_GENERATOR}
    CMAKE_COMMAND cmd /C "${CMAKE_CURRENT_BINARY_DIR}/cmake_command.bat" 
)

ExternalProject_Add(aws_c_common_3rdparty
    GIT_REPOSITORY https://github.com/awslabs/aws-c-common
    GIT_TAG v0.4.29
    SOURCE_DIR ${EXT_SOURCE_DIR}/aws-c-common
    BINARY_DIR ${EXT_BUILD_DIR}/aws-c-common
    CMAKE_ARGS
        -DCMAKE_C_COMPILER=${C_COMPILER}
        -DCMAKE_CXX_COMPILER=${CXX_COMPILER}
        -G${CMAKE_GENERATOR}
        -DCMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE}
        "-DCMAKE_INSTALL_PREFIX=${EXT_INSTALL_DIR}"
        "-DCMAKE_PREFIX_PATH=${EXT_INSTALL_DIR}"
        "-DCMAKE_C_FLAGS=-w -fexceptions"
        "-DCMAKE_CXX_FLAGS=-w -fcxx-exceptions"
        -DBUILD_SHARED_LIBS=${BUILD_SHARED_LIBS}
    CMAKE_GENERATOR ${CMAKE_GENERATOR}
    CMAKE_COMMAND cmd /C "${CMAKE_CURRENT_BINARY_DIR}/cmake_command.bat" 
)

ExternalProject_Add(aws_c_event_stream_3rdparty
    GIT_REPOSITORY https://github.com/awslabs/aws-c-event-stream
    GIT_TAG v0.1.4
    SOURCE_DIR ${EXT_SOURCE_DIR}/aws-c-event-stream
    BINARY_DIR ${EXT_BUILD_DIR}/aws-c-event-stream
    CMAKE_ARGS
        -DCMAKE_C_COMPILER=${C_COMPILER}
        -DCMAKE_CXX_COMPILER=${CXX_COMPILER}
        -G${CMAKE_GENERATOR}
        -DCMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE}
        "-DCMAKE_INSTALL_PREFIX=${EXT_INSTALL_DIR}"
        "-DCMAKE_PREFIX_PATH=${EXT_INSTALL_DIR}"
        "-DCMAKE_C_FLAGS=-w -fexceptions"
        "-DCMAKE_CXX_FLAGS=-w -fcxx-exceptions"
        -DBUILD_SHARED_LIBS=${BUILD_SHARED_LIBS}
    CMAKE_GENERATOR ${CMAKE_GENERATOR}
    CMAKE_COMMAND cmd /C "${CMAKE_CURRENT_BINARY_DIR}/cmake_command.bat" 
)

ExternalProject_Add(aws_checksums_3rdparty
    GIT_REPOSITORY https://github.com/awslabs/aws-checksums
    GIT_TAG v0.1.5
    SOURCE_DIR ${EXT_SOURCE_DIR}/aws-checksums
    BINARY_DIR ${EXT_BUILD_DIR}/aws-checksums
    CMAKE_ARGS
        -DCMAKE_C_COMPILER=${C_COMPILER}
        -DCMAKE_CXX_COMPILER=${CXX_COMPILER}
        -G${CMAKE_GENERATOR}
        -DCMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE}
        "-DCMAKE_INSTALL_PREFIX=${EXT_INSTALL_DIR}"
        "-DCMAKE_PREFIX_PATH=${EXT_INSTALL_DIR}"
        "-DCMAKE_C_FLAGS=-w -fexceptions"
        "-DCMAKE_CXX_FLAGS=-w -fcxx-exceptions"
        -DBUILD_SHARED_LIBS=${BUILD_SHARED_LIBS}
    CMAKE_GENERATOR ${CMAKE_GENERATOR}
    CMAKE_COMMAND cmd /C "${CMAKE_CURRENT_BINARY_DIR}/cmake_command.bat" 
)

add_dependencies(aws_c_event_stream_3rdparty aws_c_common_3rdparty aws_checksums_3rdparty)