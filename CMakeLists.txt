cmake_minimum_required(VERSION 3.15)
project(tensorflow)

set(SOLUTION_NAME ${PROJECT_NAME})
list(APPEND CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/CMake)

###############################################################################################
# set options
###############################################################################################

###############################################################################################
# add subdirectory
###############################################################################################

add_subdirectory(third_party EXCLUDE_FROM_ALL)
add_subdirectory(tensorflow)

###############################################################################################
# install
###############################################################################################

#install(TARGETS ${${PROJECT_NAME}_TARGET_NAME}
#   EXPORT ${SOLUTION_NAME}-export
#   LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
#   ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
#   RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
#)
#
#install(EXPORT ${SOLUTION_NAME}-export
#    FILE
#    ${SOLUTION_NAME}Targets.cmake
#    NAMESPACE
#    ${SOLUTION_NAME}::
#    DESTINATION
#    ${CMAKE_INSTALL_DATADIR}/${SOLUTION_NAME}
#)
#
#install(FILES "${PROJECT_BINARY_DIR}/${SOLUTION_NAME}Config.cmake"
#    DESTINATION ${CMAKE_INSTALL_DATADIR}/${SOLUTION_NAME}
#)