#"C:\Program Files\CMake\bin\cmake.EXE" --no-warn-unused-cli -DCMAKE_EXPORT_COMPILE_COMMANDS:BOOL=TRUE -DCMAKE_BUILD_TYPE:STRING=Debug -DCMAKE_C_COMPILER:FILEPATH=C:\ARM_Tools\102021.10\bin\arm-none-eabi-gcc.exe -DCMAKE_CXX_COMPILER:FILEPATH=C:\ARM_Tools\102021.10\bin\arm-none-eabi-g++.exe -SC:/VsLocal3/workspace_test/project -Bc:/VsLocal3/workspace_test/project/Build -GNinja
cmake_minimum_required(VERSION 3.10)

set(CMAKE_TOOLCHAIN_FILE Toolchain-arm-gcc.cmake)

# Allow assembler
enable_language(ASM)

project(stm32l431_test)

set(SRC_DIR ${CMAKE_CURRENT_SOURCE_DIR})

if(MY_CMAKE_MODE STREQUAL "DEBUG")
	add_compile_options("-fpermissive")
elseif(MY_CMAKE_MODE STREQUAL "RELEASE")
    add_compile_options("-Wformat")
endif()

set(INCLUDE_DIRS
    ${SRC_DIR}/App/inc
    ${SRC_DIR}/Libs/CMSIS/Include
    ${SRC_DIR}/Libs/CMSIS/Device/ST/STM32L4xx/Include
    ${SRC_DIR}/Libs/STM32L4xx_HAL_Driver/Inc/
    ${SRC_DIR}/Libs/STM32L4xx_HAL_Driver/Inc/Legacy
)

set(C_SRCS
    ${SRC_DIR}/App/src/main.c
    ${SRC_DIR}/App/src/stm32l4xx_hal_msp.c
    ${SRC_DIR}/App/src/stm32l4xx_it.c
    ${SRC_DIR}/App/src/syscalls.c
    ${SRC_DIR}/App/src/sysmem.c
    ${SRC_DIR}/App/src/system_stm32l4xx.c
    ${SRC_DIR}/Libs/STM32L4xx_HAL_Driver/src/stm32l4xx_hal.c
    ${SRC_DIR}/Libs/STM32L4xx_HAL_Driver/src/stm32l4xx_hal_cortex.c
    ${SRC_DIR}/Libs/STM32L4xx_HAL_Driver/src/stm32l4xx_hal_dma.c
    ${SRC_DIR}/Libs/STM32L4xx_HAL_Driver/src/stm32l4xx_hal_dma_ex.c
    ${SRC_DIR}/Libs/STM32L4xx_HAL_Driver/src/stm32l4xx_hal_exti.c
    ${SRC_DIR}/Libs/STM32L4xx_HAL_Driver/src/stm32l4xx_hal_flash.c
    ${SRC_DIR}/Libs/STM32L4xx_HAL_Driver/src/stm32l4xx_hal_flash_ex.c
    ${SRC_DIR}/Libs/STM32L4xx_HAL_Driver/src/stm32l4xx_hal_flash_ramfunc.c
    ${SRC_DIR}/Libs/STM32L4xx_HAL_Driver/src/stm32l4xx_hal_gpio.c
    ${SRC_DIR}/Libs/STM32L4xx_HAL_Driver/src/stm32l4xx_hal_pwr.c
    ${SRC_DIR}/Libs/STM32L4xx_HAL_Driver/src/stm32l4xx_hal_pwr_ex.c
    ${SRC_DIR}/Libs/STM32L4xx_HAL_Driver/src/stm32l4xx_hal_rcc.c
    ${SRC_DIR}/Libs/STM32L4xx_HAL_Driver/src/stm32l4xx_hal_rcc_ex.c
)

set(ASM_SRCS
    ${SRC_DIR}/App/startup/startup_stm32l431rctx.s
)

set(GLOBAL_DEFINES
    -DDEBUG
    -DUSE_HAL_DRIVER
    -DSTM32L431xx
)

# Do linkowania bibliotek 
set(LINKED_LIBS
    # libLibrary.a dodaje sie tutaj samą nazwe bez lib na początku oraz .a na końcu 
)

link_directories(
    # sciezka do bibliotek statycznych 
)

include_directories(${INCLUDE_DIRS})
add_definitions(${GLOBAL_DEFINES})

include(compiler_flags.cmake)

add_arm_executable(${CMAKE_PROJECT_NAME} ${CPP_SRCS} ${C_SRCS} ${ASM_SRCS})

arm_link_libraries(${CMAKE_PROJECT_NAME} ${LINKED_LIBS})