cmake_minimum_required(VERSION 3.28)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

########################################################################
# Boilerplate for allowing the project to build on both linux/unix/mac
# and on Windows

if (WIN32)
    set(USERHOME $ENV{USERPROFILE})
else ()
    set(USERHOME $ENV{HOME})
endif ()

# Verify the pico SDK is where we expect it, and abort if it's not
if (NOT EXISTS ${USERHOME}/.pico-sdk/sdk/1.5.1)
    message(FATAL_ERROR "expected to find the pico SDK and toolchain at ${USERHOME}/.pico-sdk/sdk/1.5.1")
endif ()

set(PICO_SDK_PATH ${USERHOME}/.pico-sdk/sdk/1.5.1)
set(PICO_TOOLCHAIN_PATH ${USERHOME}/.pico-sdk/toolchain/13_2_Rel1)

if (WIN32)
    set(pico-sdk-tools_DIR ${USERHOME}/.pico-sdk/tools/1.5.1)
    include(${pico-sdk-tools_DIR}/pico-sdk-tools-config.cmake)
    include(${pico-sdk-tools_DIR}/pico-sdk-tools-config-version.cmake)
endif ()
########################################################################


include(project.cmake)

include(pico_sdk_import.cmake)

# Uncomment this line if you want the pico_extras libraries
# include(pico_extras_import_optional.cmake)

project(${PROJ_NAME} C CXX ASM)

pico_sdk_init()

add_executable(${PROJ_NAME} main.c)

pico_enable_stdio_usb(${PROJ_NAME} 1)
pico_enable_stdio_uart(${PROJ_NAME} 0)

pico_set_program_name(${PROJ_NAME} ${PROJ_NAME})
pico_set_program_version(${PROJ_NAME} ${PROJ_VER})

target_include_directories(${PROJ_NAME} PRIVATE
        include
)

include(pico_link_libs.cmake)

pico_add_extra_outputs(${PROJ_NAME})