cmake_minimum_required(VERSION 3.5)

project(lv-evse-hmi)

include(GNUInstallDirs)

set(BUILD_SHARED_LIBS GLOBAL ON)
add_compile_options(-fPIC)

set(LV_LVGL_H_INCLUDE_SIMPLE true CACHE BOOL "")
set(LIB_INSTALL_DIR "lib64"    CACHE STRING "where to install lib relative to prefix")
set(INC_INSTALL_DIR "include/lvgl" CACHE STRING "where to install headers relative to prefix")

#set(USE_GTK true CACHE BOOL "Select GTL frame-buffer emulator")
if (${USE_GTK})
  message (STATUS "*** GTK emulator selected ***")
  if (NOT DEFINED X_RES)
     set(X_RES 1024 CACHE INTEGER "emulator horizontal definition")
  endif()

  if (NOT DEFINED Y_RES)
     set(Y_RES 600 CACHE INTEGER "emulator vertical definition")
  endif()
  find_package(PkgConfig)
  pkg_check_modules(PKG_GTK gtk+-3.0 glib-2.0)
  include_directories(AFTER SYSTEM ${PKG_GTK_INCLUDE_DIRS})
  link_libraries(${PKG_GTK_LIBRARIES})
  add_compile_options(-DUSE_GTK=1 -DUSE_FBDEV=0 -DUSE_EVDEV=0 -DLV_HOR_RES_MAX=${X_RES} -DLV_VER_RES_MAX=${Y_RES})

else()

  message (STATUS "*** Frame buffer selected ***")
   set (USE_FBDEV 1 GLOBAL BOOL "")
   set (USE_EVDEV 1 GLOBAL BOOL "")
   add_compile_options(-DUSE_FBDEV=1 -DUSE_EVDEV=1 -DUSE_GTK=0)
endif(${USE_GTK})

include_directories(.)
add_subdirectory(lvgl)
add_subdirectory(lv_drivers)


configure_file("${CMAKE_CURRENT_SOURCE_DIR}/lvgl.pc.in" "${CMAKE_CURRENT_SOURCE_DIR}/lvgl.pc" @ONLY)
INSTALL(FILES
        ${CMAKE_CURRENT_SOURCE_DIR}/lvgl.pc
		DESTINATION
        ${CMAKE_INSTALL_LIBDIR}/pkgconfig
	)