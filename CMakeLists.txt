cmake_minimum_required(VERSION 3.22)

#
# This file is generated only once and is not re-generated if the converter is called multiple times.
# The user is free to modify the file as much as necessary.
#

# Setup compiler settings
set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED ON)
set(CMAKE_C_EXTENSIONS ON)

# Define the build type
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Debug")
endif()

# Set the project name
set(CMAKE_PROJECT_NAME STM32F746DISCO-TinyUSB-MSC)

# Include toolchain file for ARM GCC
include("cmake/gcc-arm-none-eabi.cmake")

# Enable compile commands to ease indexing with tools like clangd
set(CMAKE_EXPORT_COMPILE_COMMANDS TRUE)

# Enable CMake support for ASM and C languages
enable_language(C ASM)

# Core project settings
project(${CMAKE_PROJECT_NAME})
message("Build type: ${CMAKE_BUILD_TYPE}")

# Create an executable object type
add_executable(${CMAKE_PROJECT_NAME})

# Add STM32CubeMX generated sources
add_subdirectory(cmake/stm32cubemx)

# Define the TinyUSB configuration target as an INTERFACE library
add_library(tinyusb_config INTERFACE)

# Add TinyUSB include paths
target_include_directories(tinyusb_config INTERFACE 
    Core/Inc
    tinyusb/src
    tinyusb/hw
)

# Add TinyUSB directory
add_subdirectory(tinyusb/src)

# Link directories setup (if needed, add user-defined library search paths)
target_link_directories(${CMAKE_PROJECT_NAME} PRIVATE
    # Add user-defined library search paths
)

# Add sources to executable
target_sources(${CMAKE_PROJECT_NAME} PRIVATE
    # TinyUSB Example (CDC-MSC) source files
    Core/Src/msc_disk.c
    Core/Src/usb_descriptors.c
    tinyusb/src/portable/synopsys/dwc2/dcd_dwc2.c
)

# Add include paths to the main executable
target_include_directories(${CMAKE_PROJECT_NAME} PRIVATE
    # Add user-defined include paths
)

# Add project symbols (macros)
target_compile_definitions(${CMAKE_PROJECT_NAME} PRIVATE
    # Add user-defined symbols
)

# Add linked libraries
target_link_libraries(${CMAKE_PROJECT_NAME}
    stm32cubemx
    # TinyUSB Library Stack
    tinyusb_config
    tinyusb
)
