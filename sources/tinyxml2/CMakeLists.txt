cmake_minimum_required(VERSION 3.10)

set(lib_name ext_tinyxml2)

project(${lib_name})

add_library(${lib_name} STATIC
  ${CMAKE_CURRENT_LIST_DIR}/tinyxml2.cpp
)

target_include_directories(${lib_name} PUBLIC ${CMAKE_CURRENT_LIST_DIR})

if(WINDOWS)
  if(BUILD_SHARED_LIBS)
    target_compile_definitions(${lib_name} PRIVATE _USRDLL)
  else()
    target_compile_definitions(${lib_name} PRIVATE CC_STATIC)
  endif()
endif()

set_target_properties(${lib_name}
    PROPERTIES
    ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib"
    LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib"
    FOLDER "External"
)