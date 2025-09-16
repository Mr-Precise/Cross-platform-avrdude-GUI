# DaduDa
cmake_minimum_required(VERSION 3.10)

project(daduda LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

if(MSVC)
    add_compile_options(/W3)
else()
    add_compile_options(-Wall)
endif()

if(MINGW)
    add_compile_options(-frtti -fexceptions)
endif()

set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTORCC ON)

option(USE_QT6 "Build with Qt6" OFF)

if(USE_QT6)
    find_package(Qt6 REQUIRED COMPONENTS Core Gui Widgets SerialPort)
    set(QT_LIBS Qt6::Core Qt6::Gui Qt6::Widgets Qt6::SerialPort)
else()
    find_package(Qt5 REQUIRED COMPONENTS Core Gui Widgets SerialPort)
    set(QT_LIBS Qt5::Core Qt5::Gui Qt5::Widgets Qt5::SerialPort)
endif()

set(SRC_DIR source)

set(SOURCES
    ${SRC_DIR}/main.cpp
    ${SRC_DIR}/daduda.cpp
)

set(HEADERS
    ${SRC_DIR}/daduda.h
)

set(FORMS
    ${SRC_DIR}/daduda.ui
)

add_executable(${PROJECT_NAME}
    ${SOURCES}
    ${HEADERS}
    ${FORMS}
)

target_link_libraries(${PROJECT_NAME}
    PRIVATE
        Qt${QT_VERSION_MAJOR}::Core
        Qt${QT_VERSION_MAJOR}::Gui
        Qt${QT_VERSION_MAJOR}::Widgets
        Qt${QT_VERSION_MAJOR}::SerialPort
)

configure_file(
    ${PROJECT_SOURCE_DIR}/cmake/cmake_uninstall.cmake.in
    ${CMAKE_BINARY_DIR}/cmake_uninstall.cmake
    @ONLY
)

add_custom_target(uninstall
    ${CMAKE_COMMAND} -P ${CMAKE_BINARY_DIR}/cmake_uninstall.cmake
)


install(TARGETS ${PROJECT_NAME}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)

message(STATUS "Using install prefix: ${CMAKE_INSTALL_PREFIX}")
