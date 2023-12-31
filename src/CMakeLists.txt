cmake_minimum_required(VERSION 3.2.2)
project(LogAnalyzer)

#set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)
#set(CMAKE_AUTORCC ON)
set(CMAKE_INCLUDE_CURRENT_DIR ON)

find_package( Qt5Core 5.3 REQUIRED )
find_package( Qt5Widgets REQUIRED )
find_package( Qt5Gui REQUIRED )
find_package( Qt5Sql REQUIRED )
find_package( Qt5Svg REQUIRED )
find_package( Qt5PrintSupport REQUIRED )
find_package( Qt5Network REQUIRED )

qt5_wrap_ui(dialogs/ui_settingsdialog.h settingsdialog.ui)
qt5_wrap_ui(libraries/qmarkdowntextedit/ui_qplaintexteditsearchwidget.h
        libraries/qmarkdowntextedit/qplaintexteditsearchwidget.ui)

set(RESOURCE_FILES
    loganalyzer.qrc
    libraries/qmarkdowntextedit/media.qrc
)

qt5_add_resources(RESOURCE_ADDED ${RESOURCE_FILES})

set(SOURCE_FILES
    version.h
    release.h
    main.cpp
    mainwindow.cpp
    mainwindow.h
    mainwindow.ui
    qtexteditsearchwidget.cpp
    qtexteditsearchwidget.h
    utils/misc.cpp
    utils/misc.h
    services/databaseservice.cpp
    services/databaseservice.h
    entities/logfilesource.cpp
    entities/logfilesource.h
    dialogs/settingsdialog.ui
    dialogs/settingsdialog.h
    dialogs/settingsdialog.cpp
    dialogs/filedialog.cpp
    dialogs/filedialog.h
    libraries/simplecrypt/simplecrypt.h
    libraries/simplecrypt/simplecrypt.cpp
    libraries/miniz/tinfl.c
    services/cryptoservice.h
    services/cryptoservice.cpp
    services/ezpublishservice.h
    services/ezpublishservice.cpp
    libraries/qmarkdowntextedit/linenumberarea.h
    libraries/qmarkdowntextedit/qownlanguagedata.cpp
    libraries/qmarkdowntextedit/qownlanguagedata.h
    libraries/qmarkdowntextedit/markdownhighlighter.cpp
    libraries/qmarkdowntextedit/markdownhighlighter.h
    libraries/qmarkdowntextedit/qmarkdowntextedit.cpp
    libraries/qmarkdowntextedit/qmarkdowntextedit.h
    libraries/qmarkdowntextedit/qplaintexteditsearchwidget.cpp
    libraries/qmarkdowntextedit/qplaintexteditsearchwidget.h
    libraries/qmarkdowntextedit/qplaintexteditsearchwidget.ui
)

add_executable(LogAnalyzer ${SOURCE_FILES} ${RESOURCE_ADDED})


# The Qt5Widgets_INCLUDES also includes the include directories for
# dependencies QtCore and QtGui
include_directories(
        ${Qt5Widgets_INCLUDES}
        ${Qt5Sql_INCLUDES}
        ${Qt5Svg_INCLUDES}
        ${Qt5PrintSupport_INCLUDES}
        ${Qt5Network_INCLUDES}
)

#include_directories(${PROJECT_SOURCE_DIR}/libraries/miniz/.)


# We need add -DQT_WIDGETS_LIB when using QtWidgets in Qt 5.
add_definitions(
        ${Qt5Widgets_DEFINITIONS}
        ${Qt5Sql_DEFINITIONS}
        ${Qt5Svg_DEFINITIONS}
        ${Qt5PrintSupport_DEFINITIONS}
        ${Qt5Network_DEFINITIONS}
)

# Executables fail to build with Qt 5 in the default configuration
# without -fPIE. We add that here.
set(CMAKE_CXX_FLAGS "${Qt5Widgets_EXECUTABLE_COMPILE_FLAGS}")
set(CMAKE_CXX_FLAGS "${Qt5Sql_EXECUTABLE_COMPILE_FLAGS}")
set(CMAKE_CXX_FLAGS "${Qt5Svg_EXECUTABLE_COMPILE_FLAGS}")
set(CMAKE_CXX_FLAGS "${Qt5Network_EXECUTABLE_COMPILE_FLAGS}")
set(CMAKE_CXX_FLAGS "-std=c++0x")

#set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -g -Wall -Wextra")
#set(CMAKE_C_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -g -Wall -Wextra")
#set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -Wall -Wextra")
#set(CMAKE_C_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -Wall -Wextra")
#set(CMAKE_CXX_FLAGS_RELEASE "-O3")

# The Qt5Widgets_LIBRARIES variable also includes QtGui and QtCore
target_link_libraries(
        LogAnalyzer
        ${Qt5Widgets_LIBRARIES}
        ${Qt5Sql_LIBRARIES}
        ${Qt5Svg_LIBRARIES}
        ${Qt5PrintSupport_LIBRARIES}
        ${Qt5Network_LIBRARIES}
)
