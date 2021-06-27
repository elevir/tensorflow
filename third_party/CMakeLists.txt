cmake_minimum_required(VERSION 3.15)
project(third_party)

set(3RD_PARTY_INSTALL_DIR "${CMAKE_BINARY_DIR}/third_party/install")
set(CMAKE_PREFIX_PATH "${CMAKE_PREFIX_PATH};${3RD_PARTY_INSTALL_DIR}" PARENT_SCOPE)
foreach(PROJECT
    absl aws clog cpuinfo dlpack eigen3 farmhash FP16 gemmlowp protobuf)
    set(EXTERNAL_SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/${PROJECT}")
    set(EXTERNAL_BINARY_DIR "${CMAKE_BINARY_DIR}/third_party/external/${PROJECT}")

    set(3RD_PARTY_SOURCE_DIR "${CMAKE_BINARY_DIR}/third_party/source/${PROJECT}")
    set(3RD_PARTY_BUILD_DIR "${CMAKE_BINARY_DIR}/third_party/build/${PROJECT}")

    add_custom_target(${PROJECT}_3rdparty
        COMMAND ${CMAKE_COMMAND}
            -S${EXTERNAL_SOURCE_DIR}
            -B${EXTERNAL_BINARY_DIR}
            -G${CMAKE_GENERATOR}
            -DCMAKE_C_COMPILER=${CMAKE_C_COMPILER}
            -DCMAKE_CXX_COMPILER=${CMAKE_CXX_COMPILER}
            -DCMAKE_MODULE_PATH=${CMAKE_MODULE_PATH}
            -DCMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE}
            -DEXT_SOURCE_DIR=${3RD_PARTY_SOURCE_DIR}
            -DEXT_BUILD_DIR=${3RD_PARTY_BUILD_DIR}
            -DEXT_INSTALL_DIR=${3RD_PARTY_INSTALL_DIR}
            -DBUILD_SHARED_LIBS=${BUILD_SHARED_LIBS}
        COMMAND ${CMAKE_COMMAND} --build ${EXTERNAL_BINARY_DIR}
        COMMAND ${CMAKE_COMMAND} --install ${EXTERNAL_BINARY_DIR}
    )
endforeach()