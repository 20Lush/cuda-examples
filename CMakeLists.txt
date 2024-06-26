cmake_minimum_required(VERSION 3.22)

# Project version number. Good metadata to have, but ONLY INCREMENT WHEN THERE IS SOMETHING BEING RELEASED!
set(PROJECT_VERSION 1.0.0)

# Initialize our project regarding the whole repo, and give it its version.
project(CUDA_EXAMPLES_REPO
    VERSION ${PROJECT_VERSION}
    DESCRIPTION "CUDA Examples Repository"
)
message(STATUS "Repository name: " ${PROJECT_NAME})

# This runtime directory will be used for stuff that has a runtime execution part
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/runtime")

# We grab our boilerplate stuff up here since we might need it multiple times in the repo
set(CMAKE_BOILERPLATE_PATH ${CMAKE_SOURCE_DIR}/.CMake-Boilerplate)
include(${CMAKE_BOILERPLATE_PATH}/INSTALLER.cmake)
include(${CMAKE_BOILERPLATE_PATH}/VARIABLES.cmake)
include(${CMAKE_BOILERPLATE_PATH}/TESTING.cmake)

# We declare where wwe wwant our cmake module stuff to live at build-time
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_LIST_DIR}/cmake/")

# We add some compile options to make us honest and our code at least a little clean
add_compile_options(-Wall -Wextra -Wformat-overflow=0)

# In case we want to do some code coverage calculation later, lets put a coverage flag when we compile for debug
# Also set CUDAgdb flags to allow breakpoints
if(CMAKE_BUILD_TYPE STREQUAL "DEBUG")
    set(CMAKE_CXX_FLAGS "--coverage")
    set(CMAKE_CXX_OUTPUT_EXTENSION_REPLACE ON)
    add_definitions(-DDEBUG)
    set(CMAKE_CUDA_FLAGS ${CMAKE_CUDA_FLAGS} "-g -G")
    set(CUDA_NVCC_FLAGS "-g -G")
endif()


# If testing is enabled at the command line lets do the CMake part of that
if(${ENABLE_TESTING})
    enable_testing()
endif()

# There might be packages in our install path that we should use as hints later
set(PACKAGE_HINTS ${CMAKE_INSTALL_PREFIX})

# Add our subdirectories we want to build and install
add_subdirectory(executable)