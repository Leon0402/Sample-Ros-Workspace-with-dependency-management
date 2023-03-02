# ######################################################################################################################
# Preamble
# ######################################################################################################################
cmake_minimum_required(VERSION 3.25)

project(sub LANGUAGES CXX)

# ######################################################################################################################
# Project Wide Setup
# ######################################################################################################################
list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_LIST_DIR}/cmake")

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

include(GNUInstallDirs)
# ######################################################################################################################
# Dependencies
# ######################################################################################################################

include(CPM)
# cmake-format: off
CPMAddPackage(
    NAME curlpp
    GIT_REPOSITORY https://github.com/jpbarrette/curlpp.git
    GIT_TAG 2c9c5b46bc1f5d3b0ae7623bd66eea4d9426a6c5
    SYSTEM ON
)
# cmake-format: on

# ######################################################################################################################
# Main targets
# ######################################################################################################################
# Targets
add_library(sub)
add_library(sub::sub ALIAS sub)
target_sources(sub PRIVATE "src/Test.cpp")
target_include_directories(sub PUBLIC $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>)

set(SUB_INSTALL_CMAKEDIR ${CMAKE_INSTALL_LIBDIR}/cmake/sub)
install(FILES ${PROJECT_SOURCE_DIR}/cmake/configs/subConfig.cmake DESTINATION ${SUB_INSTALL_CMAKEDIR})
install(DIRECTORY include/ DESTINATION "include/sub")
install(TARGETS sub EXPORT subTargets INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})
install(
    EXPORT subTargets
    DESTINATION ${SUB_INSTALL_CMAKEDIR}
    NAMESPACE sub::
)
