# MinVRApplication
Auto build and link MinVR to your C++/ OpenGL App

This is the base of an application that uses [MinVR](https://github.com/MinVR/MinVR) as a VR toolkit to develop your 3D graphics application and port
it to muliple devices such as PC, VR headsets and CAVe multi display systems.
This project automatically downloads, builds and links MinVR to your project.  

## Prerequisites
- CMake 2.21 installed in your computer.
- C++ compiler ( visual studio, clang, gcc )
- Git
- OpenGl support

## Installation

1. Clone this repo
2. Open a terminal and go to the root folder of the project
3. use the command `cmake -S . - B .`

You need to run the command twice. The first time MinVR caches the variables to autobuild GLFW and Glew.

## How to add more MinVR plugins to your app

Go to the file `MinVR/CMakeLists.txt` and add plugins with the parameter `-DWITH_PLUGIN` as show in the example below.  For a list of available plugins, please go 
to the followng [link](https://github.com/MinVR/MinVR/tree/master/plugins)

```cmake
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
```
