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
    ${CMAKE_CURRENT_SOURCE_DIR}/dbusmenuexporterdbus_p.h
    DBusMenuExporterDBus
)

configure_file(
    dbusmenu_version.h.in
    ${CMAKE_CURRENT_BINARY_DIR}/dbusmenu_version.h
)

add_library(dbusmenu-${DBUSMENU_SUFFIX} SHARED ${dbusmenu_qt_SRCS})
set_target_properties(dbusmenu-${DBUSMENU_SUFFIX} PROPERTIES
    VERSION ${dbusmenu_qt_VERSION_MAJOR}.${dbusmenu_qt_VERSION_MINOR}
    SOVERSION ${dbusmenu_qt_VERSION_MAJOR}
)

target_link_libraries(dbusmenu-${DBUSMENU_SUFFIX}
    ${QT_QTGUI_LIBRARIES}
    ${QT_QTDBUS_LIBRARIES}
    ${QT_QTCORE_LIBRARIES}
)

generate_export_header(dbusmenu-${DBUSMENU_SUFFIX}
    BASE_NAME dbusmenu
)

install(
    TARGETS dbusmenu-${DBUSMENU_SUFFIX}
    LIBRARY DESTINATION ${LIBRARY_DESTINATION}
    ARCHIVE DESTINATION ${LIBRARY_DESTINATION}
)

install(
    DIRECTORY .
    DESTINATION ${INCLUDE_DESTINATION}
    FILES_MATCHING PATTERN "*.h"
    PATTERN "*_p.h" EXCLUDE
)

install(
    FILES
    ${CMAKE_CURRENT_BINARY_DIR}/dbusmenu_version.h
    ${CMAKE_CURRENT_BINARY_DIR}/dbusmenu_export.h
    DESTINATION ${INCLUDE_DESTINATION}
)
