#
# Copyright 2022 NXP
#
# SPDX-License-Identifier: Apache-2.0
#

zephyr_library()
zephyr_library_sources(init.c)

if(CONFIG_NXP_IMXRT_BOOT_HEADER)
  if(NOT DEFINED CONFIG_BOARD_MIMXRT1010_EVK)
    message(WARNING "It appears you are using the board definition for "
     "the MIMXRT1010-EVK, but targeting a custom board. You may need to "
     "update your flash configuration data blocks")
  endif()
  if(CONFIG_BOOT_FLEXSPI_NOR)
    # Include flash configuration block for RT1010 EVK from NXP's HAL.
    # This configuration block may need modification if another flash chip is
    # used on your custom board. See NXP AN12238 for more information.
    zephyr_compile_definitions(XIP_BOOT_HEADER_ENABLE=1)
    zephyr_compile_definitions(BOARD_FLASH_SIZE=CONFIG_FLASH_SIZE*1024)
    set(RT1010_BOARD_DIR
      "${ZEPHYR_HAL_NXP_MODULE_DIR}/mcux/mcux-sdk-ng/boards/evkmimxrt1010")
    zephyr_library_sources(${RT1010_BOARD_DIR}/xip/evkmimxrt1010_flexspi_nor_config.c)
    zephyr_library_include_directories(${RT1010_BOARD_DIR}/xip)
  endif()
endif()

if(CONFIG_MCUX_GPT_TIMER)
  message(WARNING "You appear to be using the GPT hardware timer. "
    "This timer will enable lower power modes, but at the cost of reduced "
    "hardware timer resolution")
endif()
