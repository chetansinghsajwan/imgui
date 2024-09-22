cmake_minimum_required(VERSION 3.5)
project(imgui VERSION 1.0.0)

# imgui library
file(GLOB imgui_sources "imgui/*.cpp")
file(GLOB imgui_headers "imgui/*.h")

add_library(imgui STATIC)
add_library(imgui::imgui ALIAS imgui)
target_sources(imgui PUBLIC "${imgui_sources}")
target_include_directories(imgui PUBLIC "imgui")

# imgui library install
install(
    TARGETS imgui
    EXPORT imgui_export
    DESTINATION lib
    INCLUDES DESTINATION include)

install(
    EXPORT imgui_export
    DESTINATION cmake
    NAMESPACE imgui)

install(
    FILES ${imgui_headers}
    DESTINATION include)

