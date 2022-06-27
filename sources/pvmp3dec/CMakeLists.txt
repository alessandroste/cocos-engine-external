cmake_minimum_required(VERSION 3.6)
project(pvmp3dec)

file(GLOB PVMP3DEC_SRC "${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp")

add_library(pvmp3dec STATIC ${PVMP3DEC_SRC})

target_include_directories(pvmp3dec
    PUBLIC
        ${CMAKE_CURRENT_SOURCE_DIR}/include
        ${CMAKE_CURRENT_SOURCE_DIR}/src)
