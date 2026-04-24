cmake_minimum_required(VERSION 3.19)

file(READ "${CMAKE_CURRENT_SOURCE_DIR}/wyvrn.json" WYVRN_JSON)
string(JSON WYVRN_PROJECT_VERSION GET "${WYVRN_JSON}" version)

project(nuklear VERSION ${WYVRN_PROJECT_VERSION} LANGUAGES C)

include(GNUInstallDirs)
include(CMakePackageConfigHelpers)

add_library(nuklear INTERFACE)
add_library(nuklear::nuklear ALIAS nuklear)

target_include_directories(nuklear INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>)

install(FILES "${CMAKE_CURRENT_SOURCE_DIR}/nuklear.h"
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

install(TARGETS nuklear EXPORT nuklearTargets)

install(EXPORT nuklearTargets
    FILE nuklearTargets.cmake
    NAMESPACE nuklear::
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/nuklear)

file(WRITE "${CMAKE_CURRENT_BINARY_DIR}/nuklearConfig.cmake"
    "include(\"\${CMAKE_CURRENT_LIST_DIR}/nuklearTargets.cmake\")\n")

write_basic_package_version_file(
    "${CMAKE_CURRENT_BINARY_DIR}/nuklearConfigVersion.cmake"
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY AnyNewerVersion)

install(FILES
    "${CMAKE_CURRENT_BINARY_DIR}/nuklearConfig.cmake"
    "${CMAKE_CURRENT_BINARY_DIR}/nuklearConfigVersion.cmake"
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/nuklear)
