cmake_minimum_required(VERSION 3.15)
project(absl_3rdparty)

include(ExternalProject)
include(VisualStudio)

set(EXT_SOURCE_DIR "" CACHE PATH "${CMAKE_BINARY_DIR}/source")
set(EXT_BUILD_DIR "" CACHE PATH "${CMAKE_BINARY_DIR}/build")
set(EXT_INSTALL_DIR "" CACHE PATH "${CMAKE_BINARY_DIR}/install")

file(WRITE "${CMAKE_CURRENT_BINARY_DIR}/cmake_command.bat" "call \"${VS_DEV_CMD}\" -arch=amd64 & \"${CMAKE_COMMAND}\" %*")

get_filename_component(C_COMPILER ${CMAKE_C_COMPILER} NAME)
get_filename_component(CXX_COMPILER ${CMAKE_CXX_COMPILER} NAME)

find_program(GIT git)

ExternalProject_Add(absl_3rdparty
    GIT_REPOSITORY https://github.com/abseil/abseil-cpp.git
    GIT_TAG 997aaf3a28308eba1b9156aa35ab7bca9688e9f6
    PATCH_COMMAND "${GIT}" apply "${CMAKE_CURRENT_LIST_DIR}/com_google_absl_fix_mac_and_nvcc_build.patch"
    LOG_DOWNLOAD OFF
    LOG_UPDATE OFF
    LOG_PATCH OFF
    LOG_CONFIGURE OFF
    LOG_BUILD OFF
    LOG_INSTALL OFF
    SOURCE_DIR ${EXT_SOURCE_DIR}
    BINARY_DIR ${EXT_BUILD_DIR}
    CMAKE_ARGS
        -DCMAKE_C_COMPILER=${C_COMPILER}
        -DCMAKE_CXX_COMPILER=${CXX_COMPILER}
        -GNinja
        -DCMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE}
        "-DCMAKE_INSTALL_PREFIX=${EXT_INSTALL_DIR}"
        "-DCMAKE_PREFIX_PATH=${EXT_INSTALL_DIR}"
        -DBUILD_SHARED_LIBS=${BUILD_SHARED_LIBS}
    CMAKE_GENERATOR ${CMAKE_GENERATOR}
    CMAKE_COMMAND cmd /C "${CMAKE_CURRENT_BINARY_DIR}/cmake_command.bat" 
)