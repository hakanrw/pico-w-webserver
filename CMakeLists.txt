cmake_minimum_required(VERSION 3.13)

# initialize the SDK directly
set(PICO_BOARD pico_w)
include($ENV{PICO_SDK_PATH}/pico_sdk_init.cmake)
# Pull in FreeRTOS
include($ENV{FREERTOS_KERNEL_PATH}/portable/ThirdParty/GCC/RP2040/FreeRTOS_Kernel_import.cmake)

project(webserver)

# initialize the Raspberry Pi Pico SDK
pico_sdk_init()

set(WIFI_SSID my_wifi)
set(WIFI_PASSWORD my_password)

add_definitions(-DMG_ARCH=MG_ARCH_FREERTOS -DMG_ENABLE_LWIP -DPICO_BOARD="pico_w" -DWIFI_PASSWORD="${WIFI_PASSWORD}" -DWIFI_SSID="${WIFI_SSID}" -DHTTP_URL="http://0.0.0.0")

add_executable(webserver main.c mongoose.c net.c)

target_include_directories(webserver PUBLIC .)

target_link_libraries(webserver pico_stdlib pico_stdio pico_lwip pico_cyw43_arch_lwip_sys_freertos FreeRTOS-Kernel-Heap4)

pico_add_extra_outputs(webserver)
pico_enable_stdio_usb(webserver 1)
