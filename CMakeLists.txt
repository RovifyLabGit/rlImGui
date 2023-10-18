cmake_minimum_required(VERSION 3.10)
project(rlImGui)

set(CMAKE_CXX_STANDARD 17)

# raylib (with GLFW)
set(RAYLIB_INCLUDE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/thirdparty/raylib/install/include")
if (WIN32)
    message("WINDOWS")
    set(RAYLIB_LIBRARY "${CMAKE_CURRENT_SOURCE_DIR}/thirdparty/raylib/install/lib/raylib.lib")
elseif(UNIX AND NOT APPLE)
    message("LINUX")
    set(RAYLIB_LIBRARY "${CMAKE_CURRENT_SOURCE_DIR}/thirdparty/raylib/install/lib/libraylib.a")
else()

endif()

# OpenGL
find_package(OpenGL REQUIRED)
if(OpenGL_FOUND)
    message(STATUS "OpenGL found - ${OpenGL_VERSION}")
endif()

# ImGui
add_subdirectory(thirdparty/imgui)


# rlImGui source files
set(RLIMGUI_DIR ${CMAKE_CURRENT_SOURCE_DIR})
set(RLIMGUI_SRC_FILES
    ${RLIMGUI_DIR}/rlImGui.cpp
)

add_library(${PROJECT_NAME} ${RLIMGUI_SRC_FILES})
include_directories(${RLIMGUI_DIR} ${RAYLIB_INCLUDE_DIR})
target_include_directories(${PROJECT_NAME} PUBLIC
    ${RAYLIB_INCLUDE_DIR}
    ${RLIMGUI_DIR}
)
target_link_libraries(${PROJECT_NAME} PUBLIC
    imgui
    ${RAYLIB_LIBRARY}   
    OpenGL::GL
)

if(WIN32)
    target_link_libraries(${PROJECT_NAME} PUBLIC Winmm.lib)
endif()

# Compile Options
if(MSVC)
    add_compile_options(/bigobj)
    add_definitions(-DWIN32_LEAN_AND_MEAN)
else()
    # add_compile_options(-O3 -pthread)
endif()
