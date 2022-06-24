<<<<<<< HEAD
# main cmake
cmake_minimum_required(VERSION 3.6)
project(modern_cmake)

# TODO: check why target_compile_features does work for IOS
# Options set as global for IOS
if (IOS)
    set(CMAKE_CXX_STANDARD 11)
endif()

set(CXX_STANDARD_REQUIRED ON)
set(CXX_EXTENSIONS OFF)

enable_testing()

add_subdirectory(src)
add_subdirectory(tests)
=======
cmake_minimum_required(VERSION 3.6)
project(external)

set(CC_EXTERNAL_LIBS)
set(CC_EXTERNAL_INCLUDES ${CMAKE_CURRENT_LIST_DIR}/sources)
set(CC_EXTERNAL_PRIVATE_INCLUDES)
set(CC_EXTERNAL_PRIVATE_DEFINITIONS)

list(APPEND CC_EXTERNAL_PRIVATE_DEFINITIONS
    TBB_USE_EXCEPTIONS=0 # no rtti for now
)

MESSAGE(STATUS "platform path: ${platform_spec_path}")
include(${CMAKE_CURRENT_LIST_DIR}/cmake/CocosExternalConfig.cmake)
include(${CMAKE_CURRENT_LIST_DIR}/sources/CMakeLists.txt)
if(WINDOWS OR NX_WINDOWS)
    if(${CMAKE_SIZEOF_VOID_P} STREQUAL "4")
        include(${CMAKE_CURRENT_LIST_DIR}/win32/CMakeLists.txt)
    else()
        include(${CMAKE_CURRENT_LIST_DIR}/win64/CMakeLists.txt)
    endif()
elseif(ANDROID)
    include(${CMAKE_CURRENT_LIST_DIR}/android/CMakeLists.txt)
elseif(APPLE)
    if(MACOSX)
        include(${CMAKE_CURRENT_LIST_DIR}/mac/CMakeLists.txt)
    elseif(IOS)
        # include(${CMAKE_CURRENT_LIST_DIR}/ios-m1-simulator/CMakeLists.txt) # Uncomment this line and comment next line if using M1 simulator
        include(${CMAKE_CURRENT_LIST_DIR}/ios/CMakeLists.txt)
    endif()
elseif(OHOS)
    include(${CMAKE_CURRENT_LIST_DIR}/ohos/CMakeLists.txt)
elseif(EMSCRIPTEN)
    include(${CMAKE_CURRENT_LIST_DIR}/emscripten/CMakeLists.txt)
elseif(LINUX)
    include(${CMAKE_CURRENT_LIST_DIR}/linux/CMakeLists.txt)
elseif(NX)
endif()

add_library(external empty.cpp)
target_link_libraries(external ${CC_EXTERNAL_LIBS})
target_include_directories(external PUBLIC ${CC_EXTERNAL_INCLUDES})
message("Including ${CC_EXTERNAL_LIBS}")
message("Including ${CC_EXTERNAL_INCLUDES}")
>>>>>>> 35424900 (compatibility with cocos 4)
