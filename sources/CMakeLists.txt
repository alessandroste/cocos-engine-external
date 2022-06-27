set(CC_UTILS_SOURCES
  ${CMAKE_CURRENT_LIST_DIR}/xxtea/xxtea.cpp
  ${CMAKE_CURRENT_LIST_DIR}/xxtea/xxtea.h
)

set(CC_MURMURHASH2_SOURCES
  ${CMAKE_CURRENT_LIST_DIR}/MurmurHash2/MurmurHash2.cpp
  ${CMAKE_CURRENT_LIST_DIR}/MurmurHash2/MurmurHash2.h
)

set(CC_EXTERNAL_SOURCES
  ${CMAKE_CURRENT_LIST_DIR}/tommyds/tommy.c
  ${CMAKE_CURRENT_LIST_DIR}/tommyds/tommy.h
)

# if(NOT USE_MODULES)
  list(APPEND CC_EXTERNAL_SOURCES
    ${CC_UTILS_SOURCES}
    ${CC_MURMURHASH2_SOURCES}
  )
# endif()

# add dependent boost libs
if(NOT NX)
  include(${CMAKE_CURRENT_LIST_DIR}/boost-source/boost.cmake)
endif()

if(ANDROID AND CC_ENABLE_SWAPPY)

include(${CMAKE_CURRENT_LIST_DIR}/Swappy/src/swappy/CMakeLists.txt)
list(APPEND CC_EXTERNAL_LIBS
  swappy
)
add_definitions(-DCC_SWAPPY_ENABLED)

endif()

if(ANDROID OR OHOS OR NX)
#   include(${CMAKE_CURRENT_LIST_DIR}/pvmp3dec/CMakeLists.txt)
    add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/pvmp3dec)
    include(${CMAKE_CURRENT_LIST_DIR}/tremolo/CMakeLists.txt)
    list(APPEND CC_EXTERNAL_LIBS
        pvmp3dec
        vorbisidec
    )
elseif(APPLE)
  if(USE_SOCKET)
      include(${CMAKE_CURRENT_LIST_DIR}/SocketRocket/CMakeLists.txt)
  endif()
elseif(NX_WINDOWS)
    add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/pvmp3dec)
    # include(${CMAKE_CURRENT_LIST_DIR}/pvmp3dec/CMakeLists.txt)
    list(APPEND CC_EXTERNAL_LIBS
        pvmp3dec
    )
endif()

if((NOT NX_WINDOWS) AND (NOT NX))
  list(APPEND CC_EXTERNAL_INCLUDES
  ${CMAKE_CURRENT_LIST_DIR}
  )
endif()

if(USE_PHYSICS_PHYSX)
  list(APPEND CC_EXTERNAL_INCLUDES
    ${CMAKE_CURRENT_LIST_DIR}/PhysX/PhysX/include
    ${CMAKE_CURRENT_LIST_DIR}/PhysX/PhysX/source
    ${CMAKE_CURRENT_LIST_DIR}/PhysX/PxShared/include
  )
endif()

list(APPEND CC_EXTERNAL_INCLUDES
    ${CMAKE_CURRENT_LIST_DIR}/ConvertUTF
)

add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/poly2tri)
add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/clipper)
add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/tinyxml2)
add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/rapidxml)
add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/md5)
add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/edtaa3func)
add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/xxhash)
add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/chipmunk)
add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/bullet)
add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/recast)
add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/unzip)
add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/convertUTF)

# if(WINDOWS OR NX_WINDOWS)
#     if(${CMAKE_SIZEOF_VOID_P} STREQUAL "4")
#         # Use zlib source
#         message("Adding zlib from source ${CMAKE_CURRENT_SOURCE_DIR}/sources/zlib")
#         add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/sources/zlib)
#         add_library(zlib ALIAS zlibstatic)
#         list(APPEND CC_EXTERNAL_LIBS zlibstatic)
#         target_include_directories(zlibstatic PUBLIC
#             ${CMAKE_CURRENT_SOURCE_DIR}/sources/zlib
#             ${CMAKE_CURRENT_BINARY_DIR}/sources/zlib
#         )

#         set_target_properties(zlibstatic
#             PROPERTIES
#             ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib"
#             LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib"
#             FOLDER "External"
#         )
#     endif()
# endif()

list(APPEND CC_EXTERNAL_LIBS
    poly2tri
    clipper
    tinyxml2
    rapidxml
    md5
    edtaa3func
    xxhash
    chipmunk
    bullet
    recast
    unzip
    convertUTF
)
