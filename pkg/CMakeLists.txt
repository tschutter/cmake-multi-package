# Inspired by https://github.com/shadowmint/cmake-multi-install
# The removal of CPackConfig.cmake is to cleanout stale config there.
# Seen if you change the PROJECT in package/*/CMakeLists.txt.in.

# all packages
add_custom_target(pkg)

# libanimals, libanimals-dbg
if("${CMAKE_BUILD_TYPE}" STREQUAL "Debug")
    get_target_property(TARGET_LOCATION animals DEBUG_LOCATION)
else()
    get_target_property(TARGET_LOCATION animals LOCATION)
endif()
if(APPLE)
    string(REPLACE ".dylib" ".${VERSION_MAJOR_MINOR}.dylib" TARGET_LOCATION_2 ${TARGET_LOCATION})
    string(REPLACE ".dylib" ".${VERSION_MAJOR_MINOR_PATCH_BUILD}.dylib" TARGET_LOCATION_3 ${TARGET_LOCATION})
else()
    set(TARGET_LOCATION_2 "${TARGET_LOCATION}.${VERSION_MAJOR_MINOR}")
    set(TARGET_LOCATION_3 "${TARGET_LOCATION}.${VERSION_MAJOR_MINOR_PATCH_BUILD}")
endif()
list(APPEND INSTALL_LIBS ${TARGET_LOCATION})
list(APPEND INSTALL_LIBS ${TARGET_LOCATION_2})
list(APPEND INSTALL_LIBS ${TARGET_LOCATION_3})
if("${CMAKE_BUILD_TYPE}" STREQUAL "Debug")
    set(BUILD_DIR ${CMAKE_CURRENT_BINARY_DIR}/libanimals-dbg)
    configure_file(libanimals-dbg/CMakeLists.txt.in ${BUILD_DIR}/CMakeLists.txt @ONLY)
    add_custom_target(
        pkg-libanimals-dbg
        rm -f ${BUILD_DIR}/CPackConfig.cmake
        COMMAND ${CMAKE_COMMAND} .
        COMMAND ${CMAKE_CPACK_COMMAND}
        WORKING_DIRECTORY ${BUILD_DIR}
    )
    add_dependencies(pkg pkg-libanimals-dbg)
else()
    set(BUILD_DIR ${CMAKE_CURRENT_BINARY_DIR}/libanimals)
    configure_file(libanimals/CMakeLists.txt.in ${BUILD_DIR}/CMakeLists.txt @ONLY)
    add_custom_target(
        pkg-libanimals
        rm -f ${BUILD_DIR}/CPackConfig.cmake
        COMMAND ${CMAKE_COMMAND} .
        COMMAND ${CMAKE_CPACK_COMMAND}
        WORKING_DIRECTORY ${BUILD_DIR}
    )
    add_dependencies(pkg pkg-libanimals)
endif()

# libanimals-dev
set(
    INSTALL_DOCS
    ${CMAKE_SOURCE_DIR}/doc/README.txt
)
set(
    INSTALL_HEADERS
    ${CMAKE_SOURCE_DIR}/animals/cat.h
    ${CMAKE_SOURCE_DIR}/animals/dog.h
)
set(BUILD_DIR ${CMAKE_CURRENT_BINARY_DIR}/libanimals-dev)
configure_file(libanimals-dev/CMakeLists.txt.in ${BUILD_DIR}/CMakeLists.txt @ONLY)
add_custom_target(
    pkg-libanimals-dev
    rm -f ${BUILD_DIR}/CPackConfig.cmake
    COMMAND ${CMAKE_COMMAND} .
    COMMAND ${CMAKE_CPACK_COMMAND}
    WORKING_DIRECTORY ${BUILD_DIR}
)
add_dependencies(pkg pkg-libanimals-dev)

# animals
get_target_property(TARGET_LOCATION animaltest LOCATION)
message(TL  = ${TARGET_LOCATION})
list(APPEND INSTALL_BINS ${TARGET_LOCATION})
set(BUILD_DIR ${CMAKE_CURRENT_BINARY_DIR}/animals)
configure_file(animals/CMakeLists.txt.in ${BUILD_DIR}/CMakeLists.txt @ONLY)
add_custom_target(
    pkg-animals
    rm -f ${BUILD_DIR}/CPackConfig.cmake
    COMMAND ${CMAKE_COMMAND} .
    COMMAND ${CMAKE_CPACK_COMMAND}
    WORKING_DIRECTORY ${BUILD_DIR}
)
add_dependencies(pkg pkg-animals)
