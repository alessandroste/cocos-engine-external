cmake_minimum_required(VERSION 3.10)

set(lib_name ext_edtaa3func)

add_library(${lib_name} STATIC
  ${CMAKE_CURRENT_LIST_DIR}/edtaa3func.cpp
)

target_include_directories(${lib_name} PUBLIC ${CMAKE_CURRENT_LIST_DIR})
