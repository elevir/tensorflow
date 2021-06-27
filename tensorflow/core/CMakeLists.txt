cmake_minimum_required(VERSION 3.15)
project(tensorflow-core)

###############################################################################################
# set options
###############################################################################################

################################################################################
# Set cmake variables
################################################################################

set(${PROJECT_NAME}_TARGET_NAME ${PROJECT_NAME})

###############################################################################################
# prepare protos
###############################################################################################

find_program(PROTOC protoc)
file(GLOB_RECURSE PROTO_FILES "**.proto")
set(PROTOC_OUT_DIR ${CMAKE_CURRENT_BINARY_DIR}/generated/)

file(MAKE_DIRECTORY ${PROTOC_OUT_DIR})
foreach(PROTO_FILE ${PROTO_FILES})
    string(REPLACE ${CMAKE_CURRENT_SOURCE_DIR} "" FILE_DIRECTORY ${PROTO_FILE})
    get_filename_component(FILE_DIRECTORY_FULL ${PROTO_FILE} DIRECTORY)
    execute_process(COMMAND "${PROTOC}" "-I${CMAKE_SOURCE_DIR}" "-I${FILE_DIRECTORY_FULL}" "--cpp_out=${PROTOC_OUT_DIR}" ${PROTO_FILE})
endforeach()

###############################################################################################
# set target
###############################################################################################

file(GLOB_RECURSE PROTO_SOURCE_FILES "${PROTOC_OUT_DIR}/**.cc")
file(GLOB_RECURSE SOURCE_FILES "**.cc")
#set(SOURCE_FILES )
source_group("Source files" FILES ${SOURCE_FILES})

add_library(${${PROJECT_NAME}_TARGET_NAME} ${SOURCE_FILES} ${PROTO_SOURCE_FILES})
add_library(tensorflow::${${PROJECT_NAME}_TARGET_NAME} ALIAS ${${PROJECT_NAME}_TARGET_NAME})

###############################################################################################
# target tuning
###############################################################################################

target_include_directories(${${PROJECT_NAME}_TARGET_NAME}
   PUBLIC
       $<BUILD_INTERFACE:${PROTOC_OUT_DIR}>
       $<BUILD_INTERFACE:${CMAKE_SOURCE_DIR}>
       $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
)

set_target_properties(${${PROJECT_NAME}_TARGET_NAME} 
    PROPERTIES
       CXX_STANDARD 17
       CXX_EXTENSIONS OFF
)

# target_compile_definitions(${${PROJECT_NAME}_TARGET_NAME} PRIVATE )
# target_compile_options(${${PROJECT_NAME}_TARGET_NAME} PRIVATE )
# target_link_options(${${PROJECT_NAME}_TARGET_NAME} PRIVATE )

###############################################################################################
# dependencies
###############################################################################################

target_link_libraries(${${PROJECT_NAME}_TARGET_NAME} PRIVATE tensorflow-stream-executor)

find_package(Eigen3 CONFIG REQUIRED)
target_link_libraries(${${PROJECT_NAME}_TARGET_NAME} PRIVATE Eigen3::Eigen)

find_package(absl CONFIG REQUIRED)
target_link_libraries(${${PROJECT_NAME}_TARGET_NAME} PRIVATE absl::optional)

###############################################################################################
# install
###############################################################################################

include(GNUInstallDirs)
install(DIRECTORY headers/ 
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
)
install(DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/${PUBLIC_HEADERS_DIR}"
   DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/${DEVICE_INCLUDE_DIR}
   REGEX "${CMAKE_CURRENT_BINARY_DIR}/${PRIVATE_HEADERS_DIR}" EXCLUDE
)
install(TARGETS ${${PROJECT_NAME}_TARGET_NAME}
    EXPORT ${SOLUTION_NAME}-export
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)