cmake_minimum_required(VERSION 3.21)
project(mkw_bridge LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

find_package(imgui CONFIG REQUIRED)
find_package(minhook CONFIG REQUIRED)

# Injected DLL in Dolphin
add_library(mkw_bridge SHARED
    src/mkw_bridge.cpp
)

target_include_directories(mkw_bridge PRIVATE include)

target_link_libraries(mkw_bridge PRIVATE
    imgui::imgui
    minhook::minhook
    d3d11
    dxgi
    user32
)

# Lightweight CLI injector
add_executable(injector
    src/injector.cpp
)

target_link_libraries(injector PRIVATE
    user32
    kernel32
)

if(MSVC)
  target_compile_options(mkw_bridge PRIVATE /W4 /O2 /arch:AVX2)
  target_compile_options(injector PRIVATE /W4 /O2)
endif()
