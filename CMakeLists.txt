# Copyright (c) 2023 Intel Corporation
# SPDX-License-Identifier: Apache-2.0

if(CONFIG_MONGOOSE)
    set(MONGOOSE_DIR ${ZEPHYR_BASE}/../modules/lib/mongoose)
    set(INC_DIR ${ACPI_DIR}/source/include/)
    set(SRC_DIR ${ACPI_DIR}/source)
    set(COMP_DIR ${ACPI_DIR}/source/components)
    set(PARENT_SRC_DIR ${ACPI_DIR}../../zephyr)
    set(ACPI_PARENT_DIR ${ACPI_DIR}/../)

    message(STATUS "CURRENT MODULE DIR ------------> ${ZEPHYR_CURRENT_MODULE_DIR}")
    message(STATUS "MONGOOSE MODULE DIR ------------> ${ZEPHYR_MODULE_DIR}")
    message(STATUS "MONGOOSE DIR ------------------> ${MONGOOSE_DIR}")
    message(STATUS "ZEPHYR BASE --------------> ${ZEPHYR_BASE}")

    add_definitions(-DMG_ENABLE_PACKED_FS=1)
    add_definitions(-DMG_DEVICE=2)
    add_definitions(-DMG_ARCH=7)

    add_library(MONGOOSE INTERFACE)

    add_compile_definitions(__ZEPHYR__)

    zephyr_include_directories(
        mongoose
    )

    # target_include_directories(MONGOOSE INTERFACE
    # ${MONGOOSE_DIR}
    # )
    target_link_libraries(MONGOOSE INTERFACE zephyr_interface)

    # target_compile_definitions(MONGOOSE INTERFACE LFS_CONFIG=zephyr_lfs_config.h)
    zephyr_library()

    zephyr_library_sources(
        mongoose/mongoose.c
    )

    zephyr_library_link_libraries(MONGOOSE)
endif()
