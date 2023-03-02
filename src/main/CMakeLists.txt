# ######################################################################################################################
# Preamble
# ######################################################################################################################
cmake_minimum_required(VERSION 3.25)

project(main LANGUAGES CXX)

# ######################################################################################################################
# Project Wide Setup
# ######################################################################################################################
list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_LIST_DIR}/cmake")

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

include(CPM)
# ######################################################################################################################
# Dependencies
# ######################################################################################################################
find_package(ament_cmake REQUIRED)

# Use local folder here with cpm. In practice sub repository might be somewhere else.
set(FETCHCONTENT_SOURCE_DIR_SUB "${PROJECT_SOURCE_DIR}/../sub")
# cmake-format: off
CPMAddPackage(
    NAME sub
    SYSTEM ON
    # TODO(leon): Setting build type doesn't seem to work?
    # OPTIONS "CMAKE_BUILD_TYPE Debug" "BUILD_SHARED_LIBS ON"
    OPTIONS "BUILD_SHARED_LIBS ON"
)
# cmake-format: on

# ######################################################################################################################
# Main targets
# ######################################################################################################################
# Targets
add_library(main)
add_library(main::main ALIAS main)
target_sources(main PRIVATE "src/Main.cpp")
target_include_directories(main PUBLIC $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>)
target_link_libraries(main PUBLIC sub::sub)

# Installation
install(DIRECTORY include/ DESTINATION "include/main")
install(TARGETS main EXPORT mainTargets INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})
ament_export_dependencies(sub)
ament_export_targets(mainTargets HAS_LIBRARY_TARGET)

ament_package()