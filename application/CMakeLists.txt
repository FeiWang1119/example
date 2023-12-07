cmake_minimum_required(VERSION 3.16)
project(application LANGUAGES CXX C)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_INCLUDE_CURRENT_DIR ON)

if(NOT DEFINED INSTALL_EXAMPLESDIR)
    set(INSTALL_EXAMPLESDIR "examples")
endif()

set(INSTALL_EXAMPLEDIR "${INSTALL_EXAMPLESDIR}/widgets/mainwindows/application")

include(GNUInstallDirs)
include(CMakePackageConfigHelpers)

find_package(Qt6 REQUIRED COMPONENTS Core Gui Widgets WaylandClient)
# find_package(WaylandProtocols REQUIRED)

qt_add_executable(application
    main.cpp
    mainwindow.cpp mainwindow.h
    dcontextshellwindow.cpp dcontextshellwindow.h
    qwaylandpersonalizationshellintegration.cpp qwaylandpersonalizationshellintegration_p.h
    qwaylandwindowcontextshellsurface.cpp qwaylandwindowcontextshellsurface_p.h
)

qt_generate_wayland_protocol_client_sources(application FILES
    ${CMAKE_CURRENT_SOURCE_DIR}/protocol/treeland-personalization-manager-v1.xml
)

set_target_properties(application PROPERTIES
    WIN32_EXECUTABLE TRUE
    MACOSX_BUNDLE TRUE
)

target_link_libraries(application PUBLIC
    Qt::Core
    Qt::Gui
    Qt::Widgets
    Qt::WaylandClientPrivate
)

# Resources:
set(application_resource_files
    "images/copy.png"
    "images/cut.png"
    "images/new.png"
    "images/open.png"
    "images/paste.png"
    "images/save.png"
)

qt_add_resources(application "application"
    PREFIX
        "/"
    FILES
        ${application_resource_files}
)

install(TARGETS application
    RUNTIME DESTINATION "${INSTALL_EXAMPLEDIR}"
    BUNDLE DESTINATION "${INSTALL_EXAMPLEDIR}"
    LIBRARY DESTINATION "${INSTALL_EXAMPLEDIR}"
)
