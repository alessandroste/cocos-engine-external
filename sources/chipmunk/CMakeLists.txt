project(chipmunk)

set(BUILD_STATIC ON CACHE BOOL "Build Chipmunk as a static library")
add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/src)

add_library(ext_chipmunk INTERFACE)

target_link_libraries(ext_chipmunk INTERFACE chipmunk_static)

target_include_directories(ext_chipmunk INTERFACE ${CMAKE_CURRENT_LIST_DIR}/include)