#  Copyright (c) Microsoft Corporation. All rights reserved.
#  Licensed under the MIT License.

cmake_minimum_required(VERSION 3.10)

project(HeapTracker C)

azsphere_configure_tools(TOOLS_REVISION "21.04")
azsphere_configure_api(TARGET_API_SET "9")

add_executable(${PROJECT_NAME} main.c heap_tracker_lib.c)

# Here we wrap the native memory allocation functions 
target_link_libraries(${PROJECT_NAME} applibs pthread gcc_s c -Wl,--wrap=malloc -Wl,--wrap=calloc -Wl,--wrap=free -Wl,--wrap=aligned_alloc -Wl,--wrap=realloc)

# Referencing the HardwareDefinitions directly from the SDK, so to not carry them over
azsphere_target_hardware_definition(${PROJECT_NAME} TARGET_DIRECTORY "${AZURE_SPHERE_SDK_PATH}/HardwareDefinitions" TARGET_DEFINITION "mt3620.json")

azsphere_target_add_image_package(${PROJECT_NAME})
