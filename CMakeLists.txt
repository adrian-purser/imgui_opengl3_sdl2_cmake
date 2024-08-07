cmake_minimum_required(VERSION 3.20)
project(imgui_opengl3_sdl2)

cmake_policy(SET CMP0072 NEW)
#cmake_policy(SET CMP0135 NEW)

include(FetchContent)
set(FETCHCONTENT_QUIET FALSE)

set(OpenGL_GL_PREFERENCE GLVND)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED)

#find_package(glfw3 REQUIRED)
find_package(OpenGL REQUIRED)

#==============================================================================
#	SDL2
#==============================================================================

message(STATUS "Adding SDL2...")

set( SDL2_DISABLE_SDL2MAIN ON CACHE INTERNAL "Disable SDL2Main")
set( SDL_STATIC ON CACHE BOOL "" FORCE )
set( SDL_SHARED OFF CACHE BOOL "" FORCE )

FetchContent_Declare(
    SDL2
    GIT_REPOSITORY  https://github.com/libsdl-org/SDL
#    GIT_TAG         SDL2
    GIT_TAG         origin/release-2.28.x    
    GIT_SHALLOW     TRUE
#    GIT_PROGRESS    TRUE
    OVERRIDE_FIND_PACKAGE
)

FetchContent_MakeAvailable(SDL2)

message(STATUS "SDL2 Include Dir: ${SDL2_INCLUDE_DIRS}")
message(STATUS "SDL2 Source Dir: ${SDL2_SOURCE_DIR}")
message(STATUS "SDL2 Binary Dir: ${SDL2_BINARY_DIR}")

#==============================================================================
#	IMGUI
#==============================================================================

message(STATUS "Adding ImGui...")
FetchContent_Declare(
    imgui
    GIT_REPOSITORY  https://github.com/ocornut/imgui.git
    GIT_TAG         origin/master
    GIT_SHALLOW     TRUE
#    GIT_PROGRESS    TRUE
)

FetchContent_MakeAvailable(imgui)

message(STATUS "ImGui Source Dir: ${imgui_SOURCE_DIR}")
message(STATUS "ImGui Binary Dir: ${imgui_BINARY_DIR}")

set(SOURCES
	${imgui_SOURCE_DIR}/imconfig.h
	${imgui_SOURCE_DIR}/imgui.cpp
	${imgui_SOURCE_DIR}/imgui_demo.cpp
	${imgui_SOURCE_DIR}/imgui_draw.cpp
	${imgui_SOURCE_DIR}/imgui_internal.h
	${imgui_SOURCE_DIR}/imgui_tables.cpp
	${imgui_SOURCE_DIR}/imgui_widgets.cpp
	${imgui_SOURCE_DIR}/imstb_rectpack.h
	${imgui_SOURCE_DIR}/imstb_textedit.h
	${imgui_SOURCE_DIR}/imstb_truetype.h	
	${imgui_SOURCE_DIR}/backends/imgui_impl_opengl3.cpp
	${imgui_SOURCE_DIR}/backends/imgui_impl_opengl3.h
	${imgui_SOURCE_DIR}/backends/imgui_impl_sdl2.cpp
	${imgui_SOURCE_DIR}/backends/imgui_impl_sdl2.h
	${imgui_SOURCE_DIR}/misc/cpp/imgui_stdlib.h
	${imgui_SOURCE_DIR}/misc/cpp/imgui_stdlib.cpp
)

add_library(imgui_opengl3_sdl2 STATIC ${SOURCES})
target_compile_features(imgui_opengl3_sdl2 PUBLIC cxx_std_20)
target_include_directories(imgui_opengl3_sdl2 PUBLIC ${imgui_SOURCE_DIR})
target_link_libraries(imgui_opengl3_sdl2 PUBLIC SDL2::SDL2-static)



