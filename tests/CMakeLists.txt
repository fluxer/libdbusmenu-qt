add_subdirectory(testapp)

set(test_LIBRARIES
    ${QT_QTGUI_LIBRARY}
    ${QT_QTCORE_LIBRARY}
    ${QT_QTDBUS_LIBRARY}
    ${QT_QTTEST_LIBRARY}
    dbusmenu-${QT_SUFFIX}
)

include_directories(
    ${CMAKE_SOURCE_DIR}/src
    ${CMAKE_BINARY_DIR}/src
    ${CMAKE_CURRENT_BINARY_DIR}
    ${QT_QTTEST_INCLUDE_DIR}
    ${QT_QTDBUS_INCLUDE_DIR}
)

macro(add_test_executable _executable)
    add_test(${_executable} ${_executable})
    add_executable(${_executable} ${ARGN})
    target_link_libraries(${_executable} ${test_LIBRARIES})
endmacro(add_test_executable)

add_executable(slowmenu slowmenu.cpp)
target_link_libraries(slowmenu
    ${QT_QTGUI_LIBRARIES}
    ${QT_QTDBUS_LIBRARIES}
    ${QT_QTCORE_LIBRARIES}
    dbusmenu-${QT_SUFFIX}
)

# dbusmenuexportertest
set(dbusmenuexportertest_SRCS
    dbusmenuexportertest.cpp
    testutils.cpp
)

add_test_executable(dbusmenuexportertest ${dbusmenuexportertest_SRCS})

# dbusmenuimportertest
set(dbusmenuimportertest_SRCS
    dbusmenuimportertest.cpp
    testutils.cpp
)

add_test_executable(dbusmenuimportertest ${dbusmenuimportertest_SRCS})

# dbusmenushortcuttest
set(dbusmenushortcuttest_SRCS
    dbusmenushortcuttest.cpp
)

add_test_executable(dbusmenushortcuttest ${dbusmenushortcuttest_SRCS})
