cmake_minimum_required(VERSION 3.1)

project(bitmask)

if(PROJECT_SOURCE_DIR STREQUAL CMAKE_SOURCE_DIR)
    set(BITMASK_MASTER_PROJECT ON)
endif()

set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_STANDARD 11)

if (CMAKE_CXX_COMPILER_ID MATCHES "Clang" OR CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -pedantic -Wpedantic -Wall")
endif()

option(BUILD_TESTS "Build tests" ${BITMASK_MASTER_PROJECT})

include(GNUInstallDirs)

add_library(bitmask INTERFACE)
target_sources(bitmask INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/bitmask/bitmask.hpp>
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}/bitmask/bitmask.hpp>
)

target_include_directories(bitmask INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
)

install(TARGETS bitmask EXPORT bitmaskConfig)
install(DIRECTORY include/ DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})
install(EXPORT bitmaskConfig DESTINATION share/bitmask/cmake)
export(TARGETS bitmask FILE bitmaskConfig.cmake)

if(BUILD_TESTS)
    enable_testing()
    add_subdirectory(test)
endif()
