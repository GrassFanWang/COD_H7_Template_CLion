cmake_minimum_required(VERSION 3.22)

#
# This file is generated only once,
# and is not re-generated if converter is called multiple times.
#
# User is free to modify the file as much as necessary
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
set(CMAKE_PROJECT_NAME COD_H7_Template_Clion)

# Include toolchain file
include("cmake/gcc-arm-none-eabi.cmake")

# Enable compile command to ease indexing with e.g. clangd
set(CMAKE_EXPORT_COMPILE_COMMANDS TRUE)

# Core project settings
project(${CMAKE_PROJECT_NAME})
message("Build type: " ${CMAKE_BUILD_TYPE})

# Enable CMake support for ASM and C languages
enable_language(C ASM)

# Create an executable object type
add_executable(${CMAKE_PROJECT_NAME})

# Add STM32CubeMX generated sources
add_subdirectory(cmake/stm32cubemx)

# Link directories setup
target_link_directories(${CMAKE_PROJECT_NAME} PRIVATE
    # Add user defined library search paths
)

file(GLOB_RECURSE SOURCES
        "USER/BSP/Src/*.c"
        "USER/Components/Device/Src/*.c"
        "USER/Components/Algorithm/Src/*.c"
        "USER/Components/Controller/Src/*.c"
        "USER/Application/Task/Src/*.c"
        # 可按需添加其他目录
)


# Add sources to executable
target_sources(${CMAKE_PROJECT_NAME} PRIVATE
    # Add user sources here
        ${SOURCES}
)


# Add include paths
target_include_directories(${CMAKE_PROJECT_NAME} PRIVATE
    # Add user defined include paths
        USER/Components/Device/Inc
        USER/Components/Algorithm/Inc
        USER/Components/Controller/Inc
        USER/Application/Task/Inc
        USER/BSP/Inc
        Middlewares/ST/STM32_USB_Device_Library/Core/Inc
)

# Add project symbols (macros)
target_compile_definitions(${CMAKE_PROJECT_NAME} PRIVATE
    # Add user defined symbols
)

target_link_directories(${CMAKE_PROJECT_NAME} PRIVATE
        E:/COD_H7_Template_CLion/DSP
)
# Add linked libraries
target_link_libraries(${CMAKE_PROJECT_NAME}
    stm32cubemx
    arm_cortexM7lfdp_math
        # Add user defined libraries
)

add_custom_command(
        TARGET ${CMAKE_PROJECT_NAME}
        POST_BUILD
        COMMAND ${CMAKE_OBJCOPY} ${CMAKE_PROJECT_NAME}.elf -Oihex ${CMAKE_PROJECT_NAME}.hex
        COMMENT "Generating hex file for STM32"
)

# 设置编译器选项
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -mfloat-abi=hard -mfpu=fpv5-d16")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -mfloat-abi=hard -mfpu=fpv5-d16")
set(CMAKE_ASM_FLAGS "${CMAKE_ASM_FLAGS} -mfloat-abi=hard -mfpu=fpv5-d16")

# 可选：如果需要使用DSP库
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -mthumb -mthumb-interwork -march=armv7e-m -mtune=cortex-m7")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -mthumb -mthumb-interwork -march=armv7e-m -mtune=cortex-m7")