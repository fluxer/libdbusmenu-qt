project(dbusmenu-qt)
cmake_minimum_required(VERSION 3.0.2)
set(CMAKE_AUTOMOC TRUE)
set(CMAKE_AUTOUIC TRUE)
set(CMAKE_AUTORCC TRUE)

# Build options
option(WITH_DOC "Build documentation (requires Doxygen)" ON)

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

include (CheckCXXCompilerFlag)
# Check some compiler flags
check_cxx_compiler_flag(-fvisibility=hidden __DBUSMENU_HAVE_GCC_VISIBILITY)
if (__DBUSMENU_HAVE_GCC_VISIBILITY AND NOT WIN32)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fvisibility=hidden")
endif (__DBUSMENU_HAVE_GCC_VISIBILITY AND NOT WIN32)

check_cxx_compiler_flag(-Woverloaded-virtual __DBUSMENU_HAVE_W_OVERLOADED_VIRTUAL)
if (__DBUSMENU_HAVE_W_OVERLOADED_VIRTUAL)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Woverloaded-virtual")
endif (__DBUSMENU_HAVE_W_OVERLOADED_VIRTUAL)

check_cxx_compiler_flag(-std=c++11 __DBUSMENU_HAVE_CXX11)
if (__DBUSMENU_HAVE_CXX11)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
endif (__DBUSMENU_HAVE_CXX11)

include(GNUInstallDirs)
set(LIB_DESTINATION "${CMAKE_INSTALL_LIBDIR}")
set(INCLUDE_INSTALL_DIR "include/dbusmenu-${QT_SUFFIX}")

configure_file(dbusmenu-qt.pc.in ${CMAKE_BINARY_DIR}/dbusmenu-${QT_SUFFIX}.pc @ONLY)

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

if (WITH_DOC)
    configure_file(Doxyfile.in ${CMAKE_BINARY_DIR}/Doxyfile @ONLY)

    add_custom_target(doc ALL doxygen
        WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
    )

    install(
        DIRECTORY ${CMAKE_BINARY_DIR}/html/
        DESTINATION share/doc/libdbusmenu-${QT_SUFFIX}-doc
    )
endif(WITH_DOC)
