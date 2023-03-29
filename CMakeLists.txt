cmake_minimum_required(VERSION 3.24)

project(Mechanical_Arm_Contorl C)

set(CMAKE_C_STANDARD 11)

file(GLOB_RECURSE SOURCES  "Drivers/*.*" "Core/Src/*.*" "Core/Inc/*.*" )

include_directories(Core/Inc
        Drivers/CMSIS
        Drivers/CMSIS/Device
        Drivers/CMSIS/Device/ST
        Drivers/CMSIS/Device/ST/STM32F1xx
        Drivers/CMSIS/Device/ST/STM32F1xx/Include
        Drivers/CMSIS/Include
        Drivers/STM32F1xx_HAL_Driver/Inc
        Drivers/STM32F1xx_HAL_Driver/Inc/Legacy
        )

add_executable(${PROJECT_NAME}.elf ${SOURCES} ${LINKER_SCRIPT})
set(CMAKE_EXE_LINKER_FLAGS
        "${CMAKE_EXE_LINKER_FLAGS} -Wl,-Map=${PROJECT_BINARY_DIR}/${PROJECT_NAME}.map")

set(HEX_FILE ${PROJECT_BINARY_DIR}/${PROJECT_NAME}.hex)
set(BIN_FILE ${PROJECT_BINARY_DIR}/${PROJECT_NAME}.bin)

add_custom_command(TARGET ${PROJECT_NAME}.elf POST_BUILD
        COMMAND ${CMAKE_OBJCOPY} -Oihex $<TARGET_FILE:${PROJECT_NAME}.elf> ${HEX_FILE}
        COMMAND ${CMAKE_OBJCOPY} -Obinary $<TARGET_FILE:${PROJECT_NAME}.elf> ${BIN_FILE}
        COMMENT "Building ${HEX_FILE}
Building ${BIN_FILE}")