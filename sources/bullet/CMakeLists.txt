cmake_minimum_required(VERSION 3.6)

set(lib_name bullet)
set(target_name ${lib_name})

project(${lib_name})

add_library(${target_name} STATIC
  btBulletCollisionAll.cpp
  btBulletDynamicsAll.cpp
  btLinearMathAll.cpp
)
target_include_directories(${target_name} PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
target_compile_definitions(${target_name} PUBLIC BT_USE_SSE_IN_API=1)
use_cocos2dx_compile_options(${target_name})

if(XCODE OR VS)
  cocos_mark_code_files(${target_name})
endif()