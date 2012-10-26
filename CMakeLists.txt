cmake_minimum_required(VERSION 2.6)

# Set the project name.
project(ANIMALS)

# Fetch info from version.h.
configure_file(
    ${CMAKE_SOURCE_DIR}/version.h
    ${CMAKE_BINARY_DIR}/version.h.cmake
    COPYONLY
)
file(STRINGS ${CMAKE_BINARY_DIR}/version.h.cmake _VERSION_MAJOR REGEX "VERSION_MAJOR")
file(STRINGS ${CMAKE_BINARY_DIR}/version.h.cmake _VERSION_MINOR REGEX "VERSION_MINOR")
file(STRINGS ${CMAKE_BINARY_DIR}/version.h.cmake _VERSION_PATCH REGEX "VERSION_PATCH")
file(STRINGS ${CMAKE_BINARY_DIR}/version.h.cmake _VERSION_BUILD REGEX "VERSION_BUILD")
string(REGEX MATCH "[1-9][0-9]*" VERSION_MAJOR ${_VERSION_MAJOR})
string(REGEX MATCH "[0-9][0-9]*" VERSION_MINOR ${_VERSION_MINOR})
string(REGEX MATCH "[0-9][0-9]*" VERSION_PATCH ${_VERSION_PATCH})
string(REGEX MATCH "[1-9][0-9]*" VERSION_BUILD ${_VERSION_BUILD})
set(VERSION_MAJOR_MINOR "${VERSION_MAJOR}.${VERSION_MINOR}")
set(VERSION_MAJOR_MINOR_PATCH_BUILD "${VERSION_MAJOR}.${VERSION_MINOR}.${VERSION_PATCH}.${VERSION_BUILD}")

# Set compiler flags.
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall")

# Add subdirectories.
add_subdirectory(animals)
add_subdirectory(testapp)

# pkg subdirectory must come last.
add_subdirectory(pkg)
