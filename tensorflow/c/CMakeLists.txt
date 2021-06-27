cmake_minimum_required(VERSION 3.15)
project(tensorflow-c)

###############################################################################################
# set options
###############################################################################################

################################################################################
# Set cmake variables
################################################################################

set(INCLUDE_ROOT "include_root")
set(${PROJECT_NAME}_TARGET_NAME ${PROJECT_NAME})
set (INCLUDE_DIR "${INCLUDE_ROOT}/")
string(TOLOWER "headers/${INCLUDE_DIR}" PUBLIC_HEADERS_DIR)
string(TOLOWER "headers/${INCLUDE_DIR}/private/" PRIVATE_HEADERS_DIR)

###############################################################################################
# set target
###############################################################################################

set(SOURCE_FILES )
source_group("Source files" FILES ${SOURCE_FILES})

add_library(${${PROJECT_NAME}_TARGET_NAME}  ${SOURCE_FILES})
add_library(tensorflow::${${PROJECT_NAME}_TARGET_NAME} ALIAS ${${PROJECT_NAME}_TARGET_NAME})

###############################################################################################
# target tuning
###############################################################################################

target_include_directories(${${PROJECT_NAME}_TARGET_NAME}
   PUBLIC
       $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/headers>
       $<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}/headers>
       $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
   PRIVATE
       $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src>
       $<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}/headers/private>
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

# find_package( CONFIG REQUIRED)
# target_include_directories(${${PROJECT_NAME}_TARGET_NAME} PRIVATE )
# target_link_libraries(${${PROJECT_NAME}_TARGET_NAME} PRIVATE )

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