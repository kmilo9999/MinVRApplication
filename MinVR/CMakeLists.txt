cmake_minimum_required (VERSION 3.15) 
project(MinVR)
include(ExternalProject)
include(GNUInstallDirs)
include(${CMAKE_SOURCE_DIR}/../macros.cmake)

message("${CMAKE_PROJECT_NAME} INSTALL_DIR_ABSOLUTE ${INSTALL_DIR_ABSOLUTE}")

set(EXTERNAL_DIR_LOCATION ${CMAKE_BINARY_DIR})  
set(EXTERNAL_DIR_INSTALL_LOCATION ${CMAKE_BINARY_DIR}/../install/)  

build_minvr_subproject(
   NAME MinVR
   URL "https://github.com/brown-ccv/MinVR.git"
   BUILD_ARGS
    -DWITH_PLUGIN_OPENGL=ON
    -DWITH_PLUGIN_GLFW=ON
    -DAUTOBUILD_DEPENDENCIES=ON
    -DCMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE}
 )