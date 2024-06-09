cmake_minimum_required(VERSION 3.14)

# Set project and target name
set(PROJECT_NAME "Template")
set(TARGET_NAME "Template")

# include pico sdk and extras
include(${CMAKE_SOURCE_DIR}/cmake/pico_env.cmake)
include(${PICO_SDK_PATH}/external/pico_sdk_import.cmake)
include(${PICO_EXTRAS_PATH}/external/pico_extras_import.cmake)

# Include project config
include(${CMAKE_SOURCE_DIR}/cmake/compiler_settings.cmake)

# Project name, version and languages
project(${PROJECT_NAME} VERSION 1.0 LANGUAGES C CXX ASM)

# init pico sdk
pico_sdk_init()

# Add directories
add_subdirectory(src)
add_subdirectory(inc)

# Setup executable
add_executable(${TARGET_NAME})
target_sources(${TARGET_NAME} PUBLIC ${SRC} ${INC})
target_link_libraries(${TARGET_NAME} pico_stdlib pico_unique_id)

# Link stdio to usb serial
pico_enable_stdio_usb(${TARGET_NAME} 1)
pico_enable_stdio_uart(${TARGET_NAME} 1)

# Generate uf2 file for flashing
pico_add_extra_outputs(${TARGET_NAME})
