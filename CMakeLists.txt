# Converted from Makefile
# Intended for use in GAM 200/250/300 projects
# Author: Yeo Jia Hao
# Date: 2024-11-12
# Description: CMakeLists.txt conversion for ImGuizmo with FetchContent setup
# This cmake file prepares the ImGuizmo library for use in a CMake project through FetchContent
# it will be uploaded into the remote repository for students to use through FetchContent


cmake_minimum_required(VERSION 3.11)
project(ImGuizmo)

# Add static library target


file(GLOB_RECURSE IMGUIZMO_SOURCES
    "*.cpp"
)

file(GLOB IMGUIZMO_HEADERS
    "*.h"
    "*.hpp"
)

add_library(ImGuizmo STATIC ${IMGUIZMO_SOURCES} ${IMGUIZMO_HEADERS})

target_include_directories(ImGuizmo PUBLIC 
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
    $<INSTALL_INTERFACE:include>
)

target_compile_features(ImGuizmo PUBLIC cxx_std_17)

include(CMakePackageConfigHelpers)

install(TARGETS ImGuizmo
    EXPORT ImGuizmoTargets
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
    RUNTIME DESTINATION bin
    INCLUDES DESTINATION include
)

install(FILES ${IMGUIZMO_HEADERS} DESTINATION include)

install(EXPORT ImGuizmoTargets
    FILE ImGuizmoTargets.cmake
    DESTINATION lib/cmake/ImGuizmo
)

write_basic_package_version_file(
    ${CMAKE_CURRENT_BINARY_DIR}/ImGuizmoConfigVersion.cmake
    VERSION 1.0
    COMPATIBILITY AnyNewerVersion
)

configure_package_config_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/cmake/ImGuizmoConfig.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/ImGuizmoConfig.cmake
    INSTALL_DESTINATION lib/cmake/ImGuizmo
)

install(
    FILES
        "${CMAKE_CURRENT_BINARY_DIR}/ImGuizmoConfig.cmake"
        "${CMAKE_CURRENT_BINARY_DIR}/ImGuizmoConfigVersion.cmake"
    DESTINATION lib/cmake/ImGuizmo
)

