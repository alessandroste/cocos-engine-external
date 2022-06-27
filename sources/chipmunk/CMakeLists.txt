cmake_minimum_required(VERSION 3.10)

project(chipmunk)
set(target_name chipmunk)

set(CP_BUILD_DEMOS OFF)
set(CP_INSTALL_DEMOS OFF)
set(CP_BUILD_SHARED OFF)
set(CP_BUILD_STATIC ON)
set(CP_INSTALL_STATIC OFF)

option(CP_USE_DOUBLES ON)
if(NOT CP_USE_DOUBLES)
  add_definitions(-DCP_USE_DOUBLES=0)
endif()

if(CMAKE_C_COMPILER_ID STREQUAL "Clang")
  option(FORCE_CLANG_BLOCKS "Force enable Clang blocks" YES)
endif()

if(NOT MSVC)
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=gnu99") # always use gnu99
  if(FORCE_CLANG_BLOCKS)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fblocks")
  endif()
  set(CMAKE_C_FLAGS_RELEASE "${CMAKE_C_FLAGS_RELEASE} -ffast-math") # extend release-profile with fast-math
  set(CMAKE_C_FLAGS_DEBUG "${CMAKE_C_FLAGS_DEBUG} -Wall") # extend debug-profile with -Wall
endif()

file(GLOB chipmunk_source_files "${CMAKE_CURRENT_SOURCE_DIR}/src/*.c")

add_library(${target_name} STATIC
  ${chipmunk_source_files}
)

target_include_directories(${target_name} 
  PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/include"
)

# Tell MSVC to compile the code as C++.
if(MSVC)
  set_source_files_properties(${chipmunk_source_files} PROPERTIES LANGUAGE CXX)
  set_target_properties(${target_name} PROPERTIES LINKER_LANGUAGE CXX)
endif()

use_cocos2dx_compile_options(${target_name})