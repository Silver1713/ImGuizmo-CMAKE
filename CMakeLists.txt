# Converted from Makefile
# Intended for use in GAM 200/250/300 projects
# Author: Yeo Jia Hao
# Date: 2024-11-12
# Description: CMakeLists.txt conversion for ImGuizmo with FetchContent setup

cmake_minimum_required(VERSION 3.0)
project(ImGuizmo)


set(ImGuizmo_SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR})

file(GLOB_RECURSE SOURCES "*.cpp" "*.c" "*.hpp")
file(GLOB_RECURSE HEADERS "*.h" "*.hpp")

add_library(ImGuizmo STATIC ${SOURCES} ${HEADERS})

target_include_directories(ImGuizmo PUBLIC
    $<BUILD_INTERFACE:${ImGuizmo_SOURCE_DIR}>
    $<INSTALL_INTERFACE:include>
)

export(TARGETS ImGuizmo NAMESPACE ImGuizmo:: FILE ImGuizmoConfig.cmake)

install(TARGETS ImGuizmo
    EXPORT ImGuizmoTargets
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
    RUNTIME DESTINATION bin
)
install(DIRECTORY ${ImGuizmo_SOURCE_DIR}/
    DESTINATION include/ImGuizmo
    FILES_MATCHING PATTERN "*.h" PATTERN "*.hpp"
)
install(EXPORT ImGuizmoTargets
    NAMESPACE ImGuizmo::
    DESTINATION lib/cmake/ImGuizmo
)
find_library(imgui imgui)
if (imgui)
    target_link_libraries(ImGuizmo PRIVATE imgui)
else() 
    message(FATAL_ERROR "ImGuizmo requires imgui to be defined")
endif()

