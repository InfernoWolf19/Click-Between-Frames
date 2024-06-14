cmake_minimum_required(VERSION 3.21)
project(ClickBetweenFrames VERSION 1.0.0)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_OSX_ARCHITECTURES "x86_64")
set(CMAKE_CXX_VISIBILITY_PRESET hidden)

# Set up the mod binary
add_library(${PROJECT_NAME} SHARED
    "src/main.cpp"
    # Add your cpp files here
)

# Find Geode SDK
if (NOT DEFINED ENV{GEODE_SDK})
    message(FATAL_ERROR "Unable to find Geode SDK! Please define GEODE_SDK environment variable to point to Geode")
else()
    message(STATUS "Found Geode: $ENV{GEODE_SDK}")
endif()

# Add Geode SDK as a subdirectory
add_subdirectory($ENV{GEODE_SDK} ${CMAKE_CURRENT_BINARY_DIR}/geode)

# Include directories for CustomKeybindsTest
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/CustomKeybindsTest)

# Set up dependencies, resources, link Geode
setup_geode_mod(${PROJECT_NAME})

# Link CustomKeybindsTest library or sources
target_sources(${PROJECT_NAME} PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/CustomKeybindsTest/keybind.hpp
)
})
