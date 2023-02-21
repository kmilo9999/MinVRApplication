#Using 3.19 to get a modern version of FindOpenGL.cmake
cmake_minimum_required (VERSION 3.19) 

project(MyOpenGLC++)

set(CMAKE_PREFIX_PATH ${CMAKE_BINARY_DIR}/install_${CMAKE_SYSTEM_NAME})

include(ExternalProject)
include(GNUInstallDirs)

message("CMAKE_SYSTEM_NAME ${CMAKE_SYSTEM_NAME}")

if (CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT OR "${CMAKE_INSTALL_PREFIX}" STREQUAL "")
    set (CMAKE_INSTALL_PREFIX "${CMAKE_BINARY_DIR}/install_${CMAKE_SYSTEM_NAME}" CACHE PATH "default install path" FORCE )
endif()

message("CMAKE_INSTALL_PREFIX ${CMAKE_INSTALL_PREFIX}")

list(INSERT CMAKE_PREFIX_PATH 0 ${CMAKE_INSTALL_PREFIX})
list(APPEND CMAKE_MODULE_PATH  ${CMAKE_SOURCE_DIR}/cmake ${CMAKE_INSTALL_PREFIX})


set(CMAKE_CXX_STANDARD 17)
    if(COMMAND cmake_policy)
      cmake_policy(SET CMP0003 NEW)
    endif(COMMAND cmake_policy)
add_definitions(-D_HAS_STD_BYTE=0)

if (UNIX)
	#add_compile_options(-lpthread)
  #add_definitions(-D_GLIBCXX_USE_CXX11_ABI=0)
  message(STATUS "ENTER HEEEEEEEEEEEEEEEEEREEEE222")
	SET(CMAKE_CXX_FLAGS ${CMAKE_CXX_FLAGS} "-std=c++17 -pthread ")
endif (UNIX)

set(DEFAULT_BUILD_COMMAND cmake -G "${CMAKE_GENERATOR}" . )

if(NOT CMAKE_BUILD_TYPE )
 set(CMAKE_BUILD_TYPE Debug)
endif()


message("CMAKE_BUILD_TYPE ${CMAKE_BUILD_TYPE}")

if(NOT DEBUG_SUFFIX AND CMAKE_BUILD_TYPE MATCHES "Debug")
 set(CMAKE_DEBUG_SUFFIX "d")
endif()

message("DEBUG_SUFFIX ${DEBUG_SUFFIX}")

get_filename_component(INSTALL_DIR_ABSOLUTE
  ${CMAKE_INSTALL_PREFIX} ABSOLUTE BASE_DIR ${CMAKE_CURRENT_BINARY_DIR})
  
message("MAIN INSTALL_DIR_ABSOLUTE ${INSTALL_DIR_ABSOLUTE}")

find_package(OpenGL REQUIRED)

# #GLEW
# # Configure external project
# IF(NOT EXISTS ${CMAKE_SOURCE_DIR}/glew/glew)
# execute_process(
#     COMMAND ${CMAKE_COMMAND} 
#     -DINSTALL_DIR_ABSOLUTE=${INSTALL_DIR_ABSOLUTE} 
#     -DDEFAULT_BUILD_COMMAND=${DEFAULT_BUILD_COMMAND} 
#     -DCMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE} 
#     ${CMAKE_SOURCE_DIR}/glew
#     WORKING_DIRECTORY ${CMAKE_BINARY_DIR}/glew 
# )

# execute_process( COMMAND ${CMAKE_COMMAND} --build ${CMAKE_BINARY_DIR}/glew  --config Debug )
# execute_process( COMMAND ${CMAKE_COMMAND} --build ${CMAKE_BINARY_DIR}/glew  --config Release ) 

# ENDIF()

# find_package(GLEW REQUIRED)
# if(GLEW_FOUND)
#   message(STATUS "glew found")
#   message(STATUS "GLEW_INCLUDE_DIR ${GLEW_INCLUDE_DIR}")
#   message(STATUS "GLEW_LIBRARY ${GLEW_LIBRARY}")
#  else()
#   message(STATUS "glew NOT found" )
# endif()

# #glfw
# #Configure external project
# IF(NOT EXISTS ${CMAKE_SOURCE_DIR}/glfw/glfw)
# execute_process(
#     COMMAND ${CMAKE_COMMAND}
#     -DINSTALL_DIR_ABSOLUTE=${INSTALL_DIR_ABSOLUTE} 
#     -DDEFAULT_BUILD_COMMAND=${DEFAULT_BUILD_COMMAND} 
#     -DCMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE}
#     ${CMAKE_SOURCE_DIR}/glfw
#     WORKING_DIRECTORY ${CMAKE_BINARY_DIR}/glfw
# )

#MinVR
# Configure external project
#IF(NOT EXISTS ${CMAKE_SOURCE_DIR}/MinVR/MinVR)
 execute_process(
     COMMAND ${CMAKE_COMMAND} 
     -DMINVR_PATCH_PATH=${MINVR_PATCH_PATH} 
     -DCMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX} 
     -DINSTALL_DIR_ABSOLUTE=${INSTALL_DIR_ABSOLUTE} 
     -DDEFAULT_BUILD_COMMAND=${DEFAULT_BUILD_COMMAND} 
     -DCMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE}
     ${CMAKE_SOURCE_DIR}/MinVR
     WORKING_DIRECTORY ${CMAKE_BINARY_DIR}/MinVR
 )

 
#Build external project
execute_process( COMMAND ${CMAKE_COMMAND} 
  --build ${CMAKE_BINARY_DIR}/MinVR 
  --config ${CMAKE_BUILD_TYPE} 
  
  )


#ENDIF()


find_package(GLEW REQUIRED)
if(GLEW_FOUND)
  message(STATUS "glew found")
  message(STATUS "GLEW_INCLUDE_DIRS ${GLEW_INCLUDE_DIRS}")
  message(STATUS "GLEW_LIBRARY ${GLEW_LIBRARY}")
 else()
  message(STATUS "glew NOT found" )
endif()

find_package(MinVR REQUIRED)
if(MinVR_FOUND)
  message(STATUS "MinVR found")
  message(STATUS "MinVR_INCLUDE_DIR ${MinVR_INCLUDE_DIR}")
 else()
  message(STATUS "MINNVR NOT found" )
endif()


 set(source_files
    main.cpp
)

include_directories(
    ${OPENGL_INCLUDE_DIR}
    ${MinVR_INCLUDE_DIR}
    ${GLEW_INCLUDE_DIRS}
)

set(LIBRARIES
    ${OPENGL_LIBRARY}
  ${GLEW_LIBRARY}
   ${MinVR_LIBRARY} 
)

set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/install_${CMAKE_SYSTEM_NAME}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/install_${CMAKE_SYSTEM_NAME}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/install_${CMAKE_SYSTEM_NAME}/bin)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_INSTALL_PREFIX}/bin/$<0:>)

# Setup dependency on GLEW (not needed on OSX):
add_executable(${PROJECT_NAME} ${source_files} )
target_include_directories(${PROJECT_NAME} PUBLIC ${CMAKE_INSTALL_PREFIX}/include/MinVR-2.0)
target_link_libraries(${PROJECT_NAME} PUBLIC ${MinVR_LIBRARY}  GLEW::glew )