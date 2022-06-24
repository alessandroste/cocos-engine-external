cmake_minimum_required(VERSION 3.6)

set(lib_name rapidxml)
set(target_name ${lib_name})

project(${lib_name})

add_library(${target_name} INTERFACE)

target_include_directories(${target_name} INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
)