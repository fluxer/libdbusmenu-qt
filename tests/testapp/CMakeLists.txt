set(qtapp_SRCS
    main.cpp
)

add_executable(dbusmenubench-qtapp ${qtapp_SRCS})

include_directories(
    ${CMAKE_SOURCE_DIR}/src
    ${CMAKE_BINARY_DIR}/src
    ${QT_INCLUDE_DIR}
    ${QT_QTCORE_INCLUDE_DIR}
    ${QT_QTGUI_INCLUDE_DIR}
    ${QT_QTDBUS_INCLUDE_DIR}
)

target_link_libraries(dbusmenubench-qtapp
    dbusmenu-${DBUSMENU_SUFFIX}
    ${QT_QTGUI_LIBRARY}
    ${QT_QTCORE_LIBRARY}
    ${QT_QTDBUS_LIBRARY}
)
