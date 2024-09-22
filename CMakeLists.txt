cmake_minimum_required(VERSION 3.25)
project(imgui VERSION 1.0.0)
enable_language(CXX)

# --------------------------------------------------------------------------------------------------
# options
# --------------------------------------------------------------------------------------------------
option(imgui_install              "Enable this to install imgui"            ON)
option(imgui_backend_allegro5     "Enable this to add backend allegro5"     OFF)
option(imgui_backend_android      "Enable this to add backend android"      OFF)
option(imgui_backend_dx9          "Enable this to add backend dx9"          OFF)
option(imgui_backend_dx10         "Enable this to add backend dx10"         OFF)
option(imgui_backend_dx11         "Enable this to add backend dx11"         OFF)
option(imgui_backend_dx12         "Enable this to add backend dx12"         OFF)
option(imgui_backend_glfw         "Enable this to add backend glfw"         OFF)
option(imgui_backend_glut         "Enable this to add backend glut"         OFF)
option(imgui_backend_metal        "Enable this to add backend metal"        OFF)
option(imgui_backend_opengl2      "Enable this to add backend opengl2"      OFF)
option(imgui_backend_opengl3      "Enable this to add backend opengl3"      OFF)
option(imgui_backend_osx          "Enable this to add backend osx"          OFF)
option(imgui_backend_sdl2         "Enable this to add backend sdl2"         OFF)
option(imgui_backend_sdl3         "Enable this to add backend sdl3"         OFF)
option(imgui_backend_sdlrenderer2 "Enable this to add backend sdlrenderer2" OFF)
option(imgui_backend_sdlrenderer3 "Enable this to add backend sdlrenderer3" OFF)
option(imgui_backend_vulkan       "Enable this to add backend vulkan"       OFF)
option(imgui_backend_wgpu         "Enable this to add backend wgpu"         OFF)
option(imgui_backend_win32        "Enable this to add backend win32"        OFF)

# --------------------------------------------------------------------------------------------------
# imgui library
# --------------------------------------------------------------------------------------------------
file(GLOB imgui_sources "imgui/*.cpp")
file(GLOB imgui_headers "imgui/*.h")

add_library(imgui STATIC)
target_sources(imgui PRIVATE ${imgui_sources} ${imgui_headers})
target_include_directories(imgui PRIVATE imgui)

list(APPEND imgui_targets "imgui")

# --------------------------------------------------------------------------------------------------
# imgui backends
#
# @todo add dependency requirements for each backend
# --------------------------------------------------------------------------------------------------
function(imgui_add_backend name)

    set(backend "imgui_backend_${name}")
    set(backend_sources "imgui/backends/imgui_impl_${name}.cpp")
    set(backend_headers "imgui/backends/imgui_impl_${name}.h")

    add_library(${backend})
    target_sources(${backend} PRIVATE ${backend_sources})
    target_include_directories(${backend} PRIVATE "imgui")

    list(APPEND imgui_targets ${backend})
    list(APPEND imgui_backend_headers ${backend_headers})
    list(APPEND imgui_components ${name})

    return(PROPAGATE
        imgui_targets
        imgui_backend_headers
        imgui_components)

endfunction()

if (imgui_backend_allegro5)
    imgui_add_backend(allegro5)
endif()

if (imgui_backend_android)
    imgui_add_backend(android)
endif()

if (imgui_backend_dx9)
    imgui_add_backend(dx9)
endif()

if (imgui_backend_dx10)
    imgui_add_backend(dx10)
endif()

if (imgui_backend_dx11)
    imgui_add_backend(dx11)
endif()

if (imgui_backend_dx12)
    imgui_add_backend(dx12)
endif()

if (imgui_backend_glfw)
    find_package(glfw3 REQUIRED)

    imgui_add_backend(glfw)
endif()

if (imgui_backend_glut)
    imgui_add_backend(glut)
endif()

if (imgui_backend_metal)
    imgui_add_backend(metal)
endif()

if (imgui_backend_opengl2)
    imgui_add_backend(opengl2)
endif()

if (imgui_backend_opengl3)
    imgui_add_backend(opengl3)
endif()

if (imgui_backend_osx)
    imgui_add_backend(osx)
endif()

if (imgui_backend_sdl2)
    imgui_add_backend(sdl2)
endif()

if (imgui_backend_sdl3)
    imgui_add_backend(sdl3)
endif()

if (imgui_backend_sdlrenderer2)
    imgui_add_backend(sdlrenderer2)
endif()

if (imgui_backend_sdlrenderer3)
    imgui_add_backend(sdlrenderer3)
endif()

if (imgui_backend_vulkan)
    imgui_add_backend(vulkan)
endif()

if (imgui_backend_wgpu)
    imgui_add_backend(wgpu)
endif()

if (imgui_backend_win32)
    imgui_add_backend(win32)
endif()

# --------------------------------------------------------------------------------------------------
# imgui install
# --------------------------------------------------------------------------------------------------

if (imgui_install)
    install(
        TARGETS ${imgui_targets}
        EXPORT imgui_export
        DESTINATION "lib")

    install(
        EXPORT imgui_export
        FILE "imgui-config.cmake"
        DESTINATION "cmake")

    install(
        FILES ${imgui_headers}
        DESTINATION "include")

    install(
        FILES ${imgui_backend_headers}
        DESTINATION "include/backends")
endif()
