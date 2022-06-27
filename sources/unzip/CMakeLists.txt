cmake_minimum_required(VERSION 3.6)

project(unzip)

add_library(unzip STATIC
    ioapi.cpp
    unzip.cpp
    ioapi_mem.cpp
)

target_include_directories(unzip PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

# if (ANDROID)
#     find_package(ZLIB REQUIRED)
#     target_link_libraries(unzip ZLIB::ZLIB)
# else()
    target_link_libraries(unzip zlib)
# endif()