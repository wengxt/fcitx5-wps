cmake_minimum_required(VERSION 3.6)
project(fcitx5-wps)

find_package(ECM REQUIRED 1.0.0)

set(CMAKE_MODULE_PATH  "${ECM_MODULE_PATH}" "${CMAKE_CURRENT_SOURCE_DIR}/cmake" "${CMAKE_MODULE_PATH}")
set(WPS_LIBRARY_DIR "/usr/lib/office6" CACHE STRING "Path to WPS Library")

file(GLOB qtim_fcitx_SRCS fcitx5-qt/qt4/dbusaddons/*.cpp fcitx5-qt/qt4/inputcontext/*.cpp)

include(GenerateExportHeader)

find_package(Qt4 REQUIRED)
find_package(XKBCommon REQUIRED COMPONENTS XKBCommon)
find_package(X11 REQUIRED)
find_package(PkgConfig REQUIRED)

pkg_check_modules(DBus REQUIRED IMPORTED_TARGET "dbus-1")

include_directories(
  ${QT_INCLUDE_DIR}
  ${QT_QTCORE_INCLUDE_DIR}
  ${QT_QTGUI_INCLUDE_DIR}
  ${QT_QTDBUS_INCLUDE_DIR}
  ${CMAKE_CURRENT_BINARY_DIR}
  ${CMAKE_CURRENT_SOURCE_DIR}
  )

include_directories(${X11_INCLUDE_DIRS})
link_directories(${X11_LIBRARY_DIRS})

add_library(
  qtim-fcitx MODULE
  ${qtim_fcitx_SRCS}
  )

target_include_directories(qtim-fcitx PRIVATE
    fcitx5-qt/qt4/dbusaddons
    fcitx5-qt/qt4/inputcontext
    fcitx5-qt/common
)

generate_export_header(qtim-fcitx BASE_NAME Fcitx5Qt4DBusAddons)

set_target_properties(qtim-fcitx PROPERTIES
  PREFIX ""
  COMPILE_FLAGS "-fvisibility=hidden"
  LINK_FLAGS "-Wl,--no-undefined"
  CXX_STANDARD 17
  AUTOMOC TRUE)

target_link_libraries(
  qtim-fcitx
  "${WPS_LIBRARY_DIR}/libQtCore.so"
  "${WPS_LIBRARY_DIR}/libQtDBus.so"
  XKBCommon::XKBCommon
  PkgConfig::DBus
  )

target_link_libraries(qtim-fcitx ${X11_LIBRARIES})

install(
  TARGETS qtim-fcitx
  LIBRARY DESTINATION
  "${WPS_LIBRARY_DIR}/qt/plugins/inputmethods")

