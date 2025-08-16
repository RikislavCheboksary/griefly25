cmake_minimum_required(VERSION 3.21)
project(Griefly)

option(BUILD_TESTS "Build tests" OFF)
option(BUILD_COVER "Generate test coverage" OFF)
option(BUILD_WITH_QT "Build with Qt support" ON)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/Modules/")
set(CMAKE_INSTALL_RPATH_USE_LINK_PATH TRUE)

# To avoid mark __dllexport/__dllimport all symbols
set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS TRUE)

set(UTILS_DIR utils/)
set(EXEC_DIR exec/)

set(KV_INSTALL_PATH "${CMAKE_SOURCE_DIR}/${EXEC_DIR}")

if(BUILD_WITH_QT)
    set(MINIMUM_QT_VERSION 6.0)
    # Some variables neeeded by Qt
    set(CMAKE_AUTOMOC ON)
    set(CMAKE_AUTOUIC ON)
    set(CMAKE_INCLUDE_CURRENT_DIR ON)
endif()

include(ClangFormat)

include(FilterOutList)

set(CMAKE_INSTALL_RPATH "${KV_INSTALL_PATH}")

# GTEST "search"
if(BUILD_TESTS)
    include(GoogleTest)
endif()

if (MSVC)
    add_definitions(/WX)
endif()

# Main game project
add_subdirectory(sources)

# Building launcher
if(BUILD_WITH_QT)
    add_subdirectory(utils/launcher)
endif()

# Building qzipunpacker
if(BUILD_WITH_QT)
    set(SOURCES_PATH "../../sources/")
    add_subdirectory(utils/qzip_unpacker)
endif()

add_subdirectory(utils/basic_assets_generator)
add_subdirectory(utils/map_editor)

# Building server
set(GOPATH ${CMAKE_CURRENT_SOURCE_DIR}/gopath)
add_subdirectory(${GOPATH}/src/griefly-server)
