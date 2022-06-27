cmake_minimum_required(VERSION 3.6)

set(lib_name clipper)
set(target_name ${lib_name})

project(${lib_name})

add_library(${target_name} STATIC
  clipper.cpp
)

target_include_directories(${target_name} PUBLIC ${CMAKE_CURRENT_SRC_DIR})