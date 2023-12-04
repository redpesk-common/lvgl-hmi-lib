cmake_minimum_required(VERSION 3.1)

project(lv-evse-hmi)

include(GNUInstallDirs)

set(BUILD_SHARED_LIBS OFF)
add_compile_options(-fPIC)

set(LV_LVGL_H_INCLUDE_SIMPLE true CACHE BOOL "")

include_directories(.)
add_subdirectory(lvgl)
add_subdirectory(lv_drivers)

configure_file("${CMAKE_CURRENT_SOURCE_DIR}/lvgl.pc.in" "${CMAKE_CURRENT_SOURCE_DIR}/lvgl.pc" @ONLY)

INSTALL(FILES
        ${CMAKE_CURRENT_SOURCE_DIR}/lvgl.pc
		DESTINATION
        ${CMAKE_INSTALL_LIBDIR}/pkgconfig
	)