cmake_minimum_required(VERSION 3.10)

project(myApp VERSION 1.0.0 LANGUAGES CXX)

set(DEFAULT_BUILD_TYPE "Release")
if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
  message(STATUS "Setting build type to '${DEFAULT_BUILD_TYPE}' as none was specified.")
  set(CMAKE_BUILD_TYPE "${DEFAULT_BUILD_TYPE}" CACHE STRING "Choose the type of build." FORCE)

  # Set the possible values of build type for cmake-gui
  set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "Debug" "Release" "MinSizeRel" "RelWithDebInfo")
endif()


add_executable(${PROJECT_NAME} "main.cpp")

find_package(MySharedLib REQUIRED)
target_link_libraries(${PROJECT_NAME} PUBLIC mySharedLib)

message("::: ${INTERFACE_INCLUDE_DIRECTORIES}")
