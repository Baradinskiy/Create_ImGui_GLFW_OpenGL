cmake_minimum_required(VERSION 3.31)
project(arls)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

set(IMGUI_DIR ${CMAKE_SOURCE_DIR}/include/imgui)
set(IMGUI_SOURCES
        ${IMGUI_DIR}/imgui.cpp
        ${IMGUI_DIR}/imgui_draw.cpp
        ${IMGUI_DIR}/imgui_tables.cpp
        ${IMGUI_DIR}/imgui_widgets.cpp
        ${IMGUI_DIR}/imgui_demo.cpp
        ${IMGUI_DIR}/backends/imgui_impl_glfw.cpp
        ${IMGUI_DIR}/backends/imgui_impl_opengl3.cpp
)

include(FetchContent)

FetchContent_Declare(
        glfw
        GIT_REPOSITORY https://github.com/glfw/glfw.git
        GIT_TAG        3.4
)
FetchContent_MakeAvailable(glfw)

add_library(imgui STATIC ${IMGUI_SOURCES})

target_include_directories(imgui
        PUBLIC
        ${IMGUI_DIR}
        ${IMGUI_DIR}/backends
        ${glfw_SOURCE_DIR}/include
)

find_package(OpenGL REQUIRED)

file(GLOB SOURCE src/*.cpp src/*.h)
add_executable(${PROJECT_NAME} ${SOURCE})
target_link_libraries(${PROJECT_NAME} PRIVATE imgui glfw OpenGL::GL)

