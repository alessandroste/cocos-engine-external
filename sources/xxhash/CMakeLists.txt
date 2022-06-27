cmake_minimum_required(VERSION 3.6)

set(lib_name xxhash)
set(target_name ${lib_name})

project(${lib_name})

add_library(${target_name} STATIC
  xxhash.c
)

target_include_directories(${target_name} PUBLIC .)