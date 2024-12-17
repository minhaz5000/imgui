cmake_minimum_required(VERSION 3.20)

project(
  IMGUI
  DESCRIPTION
    "Dear ImGui is a bloat-free graphical user interface library for C++"
  LANGUAGES CXX)

set(IMGUI_DIR ${CMAKE_CURRENT_SOURCE_DIR})

add_library(IMGUI ${IMGUI_DIR}/imgui.cpp ${IMGUI_DIR}/imgui_draw.cpp
                  ${IMGUI_DIR}/imgui_tables.cpp ${IMGUI_DIR}/imgui_widgets.cpp)

# option(BUILD_SHARED_LIBS "Build shared libraries" OFF)
option(IMGUI_BUILD_GLFW_OPENGL "Build GLFW and OpenGL backend (Default)" ON)

if(IMGUI_BUILD_GLFW_OPENGL)
  message(STATUS "Building GLFW and OpenGL backend")
  find_package(OpenGL REQUIRED)
  target_sources(IMGUI PRIVATE ${IMGUI_DIR}/backends/imgui_impl_glfw.cpp
                               ${IMGUI_DIR}/backends/imgui_impl_opengl3.cpp)
endif()

target_include_directories(
  IMGUI
  PUBLIC ${IMGUI_DIR}
  PUBLIC ${IMGUI_DIR}/backends)

