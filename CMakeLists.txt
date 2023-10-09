cmake_minimum_required(VERSION 3.7)
set(PROJECT OpenMugen)
project(${PROJECT})


# Add global definitions
add_definitions("-Wall")
file(GLOB_RECURSE SOURCE_FILES "src/*.cpp")

add_executable(${PROJECT} ${SOURCE_FILES})
# Using C++14
set_property(TARGET ${PROJECT} PROPERTY CXX_STANDARD 98)
set_property(TARGET ${PROJECT} PROPERTY CXX_STANDARD_REQUIRED 98)

message("CMAKE_CXX_COMPILER_ID: ${CMAKE_CXX_COMPILER_ID}
CMAKE_CXX_COMPILER_VERSION: ${CMAKE_CXX_COMPILER_VERSION}")

# gcc 5 and clang 3.4 at least to support c++14
if (CMAKE_CXX_COMPILER_ID MATCHES "Clang" AND CMAKE_CXX_COMPILER_VERSION VERSION_LESS 3.4)
    message(FATAL_ERROR "Require at least clang++-3.4")
endif ()
if (CMAKE_CXX_COMPILER_ID MATCHES "GNU" AND CMAKE_CXX_COMPILER_VERSION VERSION_LESS 5.1)
    message(FATAL_ERROR "Require at least g++-5.1")
endif ()

# see what platform we are on and set platform defines
if (${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
    add_definitions(-DGL_SILENCE_DEPRECATION)
    target_link_libraries(${PROJECT} "-framework Cocoa")
endif ()

# Find SDL
# https://cmake.org/cmake/help/latest/module/FindPkgConfig.html
INCLUDE(FindPkgConfig REQUIRED)
PKG_CHECK_MODULES(SDL REQUIRED sdl)
PKG_CHECK_MODULES(SDL_IMAGE REQUIRED SDL_image)

message("SDL_INCLUDE_DIRS: ${SDL_INCLUDE_DIRS}
SDL_IMAGE_INCLUDE_DIRS: ${SDL_IMAGE_INCLUDE_DIRS}")
include_directories(${SDL_INCLUDE_DIRS} ${SDL_IMAGE_INCLUDE_DIRS})

message("SDL_LINK_LIBRARIES: ${SDL_LINK_LIBRARIES}
SDL_IMAGE_LINK_LIBRARIES: ${SDL_IMAGE_LINK_LIBRARIES}")
target_link_libraries(${PROJECT} ${SDL_LINK_LIBRARIES} ${SDL_IMAGE_LINK_LIBRARIES})