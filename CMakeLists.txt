cmake_minimum_required(VERSION 3.14)

# Set project and target name
set(PROJECT_NAME "Template")
set(TARGET_NAME "Template")
set(LIBRARY_NAME "Template-Lib")

# Include project configs
include(${CMAKE_SOURCE_DIR}/cmake/import_config.cmake)

# Include pico sdk and extras
include(${PICO_SDK_PATH}/external/pico_sdk_import.cmake)
include(${PICO_EXTRAS_PATH}/external/pico_extras_import.cmake)

# Project name, version and languages
project(${PROJECT_NAME} VERSION 1.0 LANGUAGES C CXX ASM)

# init pico sdk
pico_sdk_init()

# Add directories
add_subdirectory(src)
add_subdirectory(inc)
add_subdirectory(lib)

if (NOT TARGET ${LIBRARY_NAME} AND NOT TARGET ${TARGET_NAME})
    # Setup library
    add_library(${LIBRARY_NAME} ${LIB_SRC} ${INC})
    target_link_libraries(${LIBRARY_NAME} pico_stdlib)
    target_include_directories(${LIBRARY_NAME} PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/src ${CMAKE_CURRENT_SOURCE_DIR}/inc)

    # Setup executable
    add_executable(${TARGET_NAME} ${SRC} ${INC})
    target_link_libraries(${TARGET_NAME} pico_stdlib pico_unique_id)

    # Add include directories
    include_directories(${CMAKE_CURRENT_SOURCE_DIR}/src)
    include_directories(${CMAKE_CURRENT_SOURCE_DIR}/inc)

    # Link stdio to usb serial
    pico_enable_stdio_usb(${TARGET_NAME} 1)
    pico_enable_stdio_uart(${TARGET_NAME} 1)

    # Generate uf2 file for flashing
    pico_add_extra_outputs(${TARGET_NAME})
endif()
