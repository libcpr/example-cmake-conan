cmake_minimum_required(VERSION 3.20)
project(cpr_example)

set(CMAKE_CXX_STANDARD 11)

# Ensure find_package can find cpr later:
list(APPEND CMAKE_MODULE_PATH ${CMAKE_BINARY_DIR})
list(APPEND CMAKE_PREFIX_PATH ${CMAKE_BINARY_DIR})

# Download the latest conan CMake integration:
if(NOT EXISTS "${CMAKE_BINARY_DIR}/conan.cmake")
   message(STATUS "Downloading conan.cmake from https://github.com/conan-io/cmake-conan")
   file(DOWNLOAD "https://raw.githubusercontent.com/conan-io/cmake-conan/master/conan.cmake"
                  "${CMAKE_BINARY_DIR}/conan.cmake")
endif()
include(${CMAKE_BINARY_DIR}/conan.cmake)

# Install cpr:
conan_cmake_run(REQUIRES cpr/1.7.2 # Replace with the latest build from: https://conan.io/center/cpr
                GENERATORS cmake_find_package)

# Find cpr:
find_package(cpr REQUIRED)
add_executable(cpr_example main.cpp)
target_link_libraries(cpr_example PRIVATE cpr::cpr)