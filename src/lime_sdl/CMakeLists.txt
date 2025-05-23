set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${PROJECT_SOURCE_DIR}/CMakeModules)

add_library(lime_sdl STATIC EXCLUDE_FROM_ALL
    config.cpp
    config.h
    default_ini.h
    emu_window/emu_window_sdl2.cpp
    emu_window/emu_window_sdl2.h
    lime_sdl.cpp
    precompiled_headers.h
    resource.h
)

if (ENABLE_SOFTWARE_RENDERER)
    target_sources(lime_sdl PRIVATE
        emu_window/emu_window_sdl2_sw.cpp
        emu_window/emu_window_sdl2_sw.h
    )
endif()
if (ENABLE_OPENGL)
    target_sources(lime_sdl PRIVATE
        emu_window/emu_window_sdl2_gl.cpp
        emu_window/emu_window_sdl2_gl.h
    )
endif()
if (ENABLE_VULKAN)
    target_sources(lime_sdl PRIVATE
        emu_window/emu_window_sdl2_vk.cpp
        emu_window/emu_window_sdl2_vk.h
    )
endif()

create_target_directory_groups(lime_sdl)

target_link_libraries(lime_sdl PRIVATE lime_common lime_core input_common network)
target_link_libraries(lime_sdl PRIVATE inih)
if (MSVC)
    target_link_libraries(lime_sdl PRIVATE getopt)
endif()
target_link_libraries(lime_sdl PRIVATE ${PLATFORM_LIBRARIES} SDL2::SDL2 Threads::Threads)

if (ENABLE_OPENGL)
    target_link_libraries(lime_sdl PRIVATE glad)
endif()

if (LIME3DS_USE_PRECOMPILED_HEADERS)
    target_precompile_headers(lime_sdl PRIVATE precompiled_headers.h)
endif()
