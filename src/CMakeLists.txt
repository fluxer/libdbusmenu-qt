include(CheckCXXSourceCompiles)

# Check whether QIcon::name() exists. It was added in late Qt 4.7 cycle, and is
# not present in betas.

set(CMAKE_REQUIRED_INCLUDES "${QT_INCLUDE_DIR}")
set(CMAKE_REQUIRED_LIBRARIES "${QT_QTGUI_LIBRARIES};${QT_QTCORE_LIBRARIES}")
check_cxx_source_compiles("
#include <QtGui/QIcon>
int main() {
    QIcon icon;
    icon.name();
    return 0;
}
" HAVE_QICON_NAME)
if (NOT HAVE_QICON_NAME)
    message(STATUS "QIcon::name() does not exist, DBusMenuExporter will not export icon names by itself")
endif()
configure_file(dbusmenu_config.h.in ${CMAKE_CURRENT_BINARY_DIR}/dbusmenu_config.h @ONLY)

set(dbusmenu_qt_SRCS
    dbusmenu_p.cpp
    dbusmenuexporter.cpp
    dbusmenuexporterdbus_p.cpp
    dbusmenuimporter.cpp
    dbusmenutypes_p.cpp
    dbusmenushortcut_p.cpp
    utils.cpp
)

include_directories(
    ${CMAKE_SOURCE_DIR}/src
    ${CMAKE_BINARY_DIR}/src
)

qt4_add_dbus_adaptor(dbusmenu_qt_SRCS
    ${CMAKE_CURRENT_SOURCE_DIR}/com.canonical.dbusmenu.xml
    ${CMAKE_CURRENT_SOURCE_DIR}/dbusmenuexporterdbus_p.h DBusMenuExporterDBus
)

configure_file(dbusmenu_version.h.in
    ${CMAKE_CURRENT_BINARY_DIR}/dbusmenu_version.h
)

add_library(dbusmenu-${QT_SUFFIX} SHARED ${dbusmenu_qt_SRCS})
set_target_properties(dbusmenu-${QT_SUFFIX} PROPERTIES
    VERSION ${dbusmenu_qt_VERSION_MAJOR}.${dbusmenu_qt_VERSION_MINOR}
    SOVERSION ${dbusmenu_qt_VERSION_MAJOR}
)

target_link_libraries(dbusmenu-${QT_SUFFIX}
    ${QT_QTGUI_LIBRARIES}
    ${QT_QTDBUS_LIBRARIES}
    ${QT_QTCORE_LIBRARIES}
)

# Make sure linking to the target adds dbusmenu-qt install directory
target_include_directories(dbusmenu-${QT_SUFFIX}
    INTERFACE "$<INSTALL_INTERFACE:${INCLUDE_INSTALL_DIR}>"
)

install(TARGETS dbusmenu-${QT_SUFFIX}
    LIBRARY DESTINATION ${LIB_DESTINATION}
    RUNTIME DESTINATION bin
)

install(DIRECTORY .
    DESTINATION ${INCLUDE_INSTALL_DIR}
    FILES_MATCHING PATTERN "*.h"
    PATTERN "*_p.h" EXCLUDE
)

install(FILES ${CMAKE_CURRENT_BINARY_DIR}/dbusmenu_version.h
    DESTINATION ${INCLUDE_INSTALL_DIR}
)
