project(dbusmenu-qt)

cmake_minimum_required(VERSION 3.3)

set(CMAKE_AUTOMOC TRUE)
set(CMAKE_C_VISIBILITY_PRESET "hidden")
set(CMAKE_CXX_VISIBILITY_PRESET "hidden")
set(CMAKE_VISIBILITY_INLINES_HIDDEN ON)

# enable symbols visibility preset for all targets
cmake_policy(SET CMP0063 NEW)

# Versions
## Package version
set(dbusmenu_qt_VERSION_MAJOR 0)
set(dbusmenu_qt_VERSION_MINOR 9)
set(dbusmenu_qt_VERSION_PATCH 2)
set(dbusmenu_qt_VERSION ${dbusmenu_qt_VERSION_MAJOR}.${dbusmenu_qt_VERSION_MINOR}.${dbusmenu_qt_VERSION_PATCH})

find_package(Katie REQUIRED)
include_directories(
    ${QT_INCLUDE_DIR}
    ${QT_QTCORE_INCLUDE_DIR}
    ${QT_QTDBUS_INCLUDE_DIR}
    ${QT_QTGUI_INCLUDE_DIR}
)
add_definitions(${QT_DEFINITIONS})

set(QT_SUFFIX "katie")

include(GenerateExportHeader)
include(GNUInstallDirs)
set(LIB_DESTINATION "${CMAKE_INSTALL_LIBDIR}")
set(INCLUDE_INSTALL_DIR "include/dbusmenu-${QT_SUFFIX}")

configure_file(
    dbusmenu-qt.pc.in
    ${CMAKE_BINARY_DIR}/dbusmenu-${QT_SUFFIX}.pc
    @ONLY
)

install(
    FILES ${CMAKE_BINARY_DIR}/dbusmenu-${QT_SUFFIX}.pc
    DESTINATION ${LIB_DESTINATION}/pkgconfig
)

add_subdirectory(src)
add_subdirectory(tools)

if (ENABLE_TESTING)
    enable_testing()
    add_subdirectory(tests)
endif()
