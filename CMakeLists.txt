cmake_minimum_required(VERSION 3.19)

project(
  EmbeddedCMake
  VERSION 0.1
  LANGUAGES ASM C)

file(GLOB_RECURSE SRC Core/*.c Core/*.s Drivers/*.c)
add_executable(EmbeddedCMake ${SRC})

target_compile_definitions(EmbeddedCMake PRIVATE $<$<CONFIG:DEBUG>:DEBUG>
                                                 USE_HAL_DRIVER STM32F746xx)

target_include_directories(
  EmbeddedCMake PRIVATE Core/Inc Drivers/CMSIS/Device/ST/STM32F7xx/Include
                        Drivers/CMSIS/Include Drivers/STM32F7xx_HAL_Driver/Inc)

target_link_libraries(
  EmbeddedCMake
  PRIVATE --specs=nano.specs -Wl,--gc-sections,-Map=${PROJECT_NAME}.map
          -T${CMAKE_CURRENT_SOURCE_DIR}/STM32F746ZGTX_FLASH.ld)
