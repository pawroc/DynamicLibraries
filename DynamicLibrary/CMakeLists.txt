cmake_minimum_required(VERSION 3.10)

project(mySharedLib VERSION 1.0.0 LANGUAGES CXX)

set(DEFAULT_BUILD_TYPE "Release")
if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
  message(STATUS "Setting build type to '${DEFAULT_BUILD_TYPE}' as none was specified.")
  set(CMAKE_BUILD_TYPE "${DEFAULT_BUILD_TYPE}" CACHE STRING "Choose the type of build." FORCE)

  # Set the possible values of build type for cmake-gui
  set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "Debug" "Release" "MinSizeRel" "RelWithDebInfo")
endif()

option(BUILD_SHARED_LIBS "Make all not specified add_libraries type to SHARED" ON)

include(GNUInstallDirs)

set(SOURCE_FILES src/mySharedLib.cpp)

add_library(${PROJECT_NAME} ${SOURCE_FILES})

target_include_directories(${PROJECT_NAME} PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include> # <prefix>/include
    PRIVATE src)

message("::: ${INTERFACE_INCLUDE_DIRECTORIES}")
set_target_properties(${PROJECT_NAME} PROPERTIES
    VERSION ${PROJECT_VERSION}
    SOVERSION 1)

# Global installation rules;
# If DESTINATION is not absolute path, it is prefixed with CMAKE_INSTALL_PREFIX
# You can change it with command `cmake -DCMAKE_INSTALL_PREFIX=<prefix_path> (...)`
install(TARGETS ${PROJECT_NAME} EXPORT MySharedLibConfig
    ARCHIVE  DESTINATION ${CMAKE_INSTALL_LIBDIR}
    LIBRARY  DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME  DESTINATION ${CMAKE_INSTALL_BINDIR})

install(DIRECTORY include/ DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/${PROJECT_NAME})
# Populate INTERFACE_INCLUDE_DIRECTORIES to all targets that link against this library
# Thanks to this there is no need to use target_include_directories with this path
# in CMakeLists of the application using this library.
target_include_directories(${PROJECT_NAME} PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/${PROJECT_NAME}>
    $<INSTALL_INTERFACE:include/${PROJECT_NAME}> # <prefix>/include
    PRIVATE src)

# Install target exported with install(TARGETS EXPORT)
install(EXPORT MySharedLibConfig DESTINATION share/MySharedLib/cmake)
