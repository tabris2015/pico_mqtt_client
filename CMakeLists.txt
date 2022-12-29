cmake_minimum_required(VERSION 3.12)

set(NAME pico_mqtt)
set(PICO_BOARD pico_w)

include(pimoroni_pico_import.cmake)
include(pico_sdk_import.cmake)

project(${NAME} C CXX ASM)
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

# Init sdk
pico_sdk_init()

include(common/pimoroni_i2c)
include(common/pimoroni_bus)
include(drivers/encoder/encoder)
include(drivers/motor/motor)
include(drivers/button/button)
include(drivers/pwm/pwm)
include(drivers/pid/pid)
include(drivers/plasma/plasma)

## pico W
set(WIFI_SSID "ZTE_2.4G_nPfgdX" CACHE INTERNAL "ssid")
set(WIFI_PASSWORD "yUUSd4Ar" CACHE INTERNAL "pass")
# Other executables

# Main application
add_executable(${NAME}
        src/main.cpp
        )

# Add lwip mqtt source
target_sources(${NAME} PRIVATE ${PICO_SDK_PATH}/lib/lwip/src/apps/mqtt/mqtt.c)
target_compile_definitions(${NAME} PRIVATE WIFI_SSID=\"${WIFI_SSID}\" WIFI_PASSWORD=\"${WIFI_PASSWORD}\")

target_include_directories(${NAME} PRIVATE include)

target_link_libraries(${NAME}
        pico_stdlib
        pico_cyw43_arch_lwip_threadsafe_background
        )

pico_add_extra_outputs(${NAME})
pico_enable_stdio_usb(${NAME} 1)
pico_enable_stdio_uart(${NAME} 0)
