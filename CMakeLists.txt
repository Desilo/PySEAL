cmake_minimum_required(VERSION 3.12)
project(seal VERSION 1.0.0 LANGUAGES CXX)
# Enable testing, but we want to show the output from google test when executing from console, so we
# add our own target. Execute with for instance `make check`.
if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release)
endif()
set(CMAKE_CXX_FLAGS "-Wall -Wextra")
set(CMAKE_CXX_FLAGS_RELEASE "-O3")

# Retrieve the standard GNU installation paths
include(GNUInstallDirs)

# Import the "download_project" function for downloading dependencies during the configure phase.
include(contrib/cmake-downloadProject/DownloadProject.cmake)

# Download microsoft/SEAL dependency during CMake configure time
# TODO: make SEAL version configurable
download_project(
    PROJ                seal
    GIT_REPOSITORY      https://github.com/microsoft/SEAL.git
    GIT_TAG             master
    GIT_SHALLOW         true
    ${UPDATE_DISCONNECTED_IF_AVAILABLE}
)

# Add seal directly to our build. 
add_subdirectory(${seal_SOURCE_DIR} ${seal_BINARY_DIR})
add_subdirectory(seal)
