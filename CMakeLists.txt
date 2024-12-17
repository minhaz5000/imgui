cmake_minimum_required(VERSION 3.20)

project(
  IMGUI
  DESCRIPTION
    "Dear ImGui is a bloat-free graphical user interface library for C++"
  LANGUAGES CXX)

set(IMGUI_DIR ${CMAKE_CURRENT_SOURCE_DIR})

# Add IMGUI library
add_library(IMGUI ${IMGUI_DIR}/imgui.cpp ${IMGUI_DIR}/imgui_draw.cpp
                  ${IMGUI_DIR}/imgui_tables.cpp ${IMGUI_DIR}/imgui_widgets.cpp)

# Option to enable GLFW and OpenGL backend
option(IMGUI_BUILD_GLFW_OPENGL "Build GLFW and OpenGL backend (Default)" ON)

if(IMGUI_BUILD_GLFW_OPENGL)
  message(STATUS "Building GLFW and OpenGL backend")

  # Ensure GLFW and OpenGL are found
  find_package(OpenGL REQUIRED)

  # Add the GLFW and OpenGL backend files
  target_sources(IMGUI PRIVATE ${IMGUI_DIR}/backends/imgui_impl_glfw.cpp
                               ${IMGUI_DIR}/backends/imgui_impl_opengl3.cpp)

  # Link GLFW and OpenGL to IMGUI
  target_link_libraries(IMGUI PUBLIC glfw OpenGL::GL)

  # Ensure GLFW's include directories are added to IMGUI
  target_include_directories(
    IMGUI PUBLIC $<TARGET_PROPERTY:glfw,INTERFACE_INCLUDE_DIRECTORIES>)
endif()

# Add the main IMGUI include directories
target_include_directories(
  IMGUI
  PUBLIC ${IMGUI_DIR}
  PUBLIC ${IMGUI_DIR}/backends)
