cmake_minimum_required(VERSION 3.10)

add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/bullet)
add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/chipmunk)
add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/clipper)
add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/ConvertUTF)
add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/edtaa3func)
add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/md5)
add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/poly2tri)
add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/tinyxml2)
add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/recast)
add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/unzip)
add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/xxhash)

list(APPEND CC_EXTERNAL_LIBS
  ext_bullet
  ext_chipmunk
  ext_convertUTF
  ext_clipper
  ext_edtaa3func
  ext_md5
  ext_poly2tri
  ext_tinyxml2
  ext_recast
  ext_unzip
  ext_xxhash
)

if(ANDROID AND CC_ENABLE_SWAPPY)
  add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/Swappy/src/swappy)
  list(APPEND CC_EXTERNAL_LIBS swappy)
  add_definitions(-DCC_SWAPPY_ENABLED)
endif()

if(ANDROID AND (NOT SURFACE_LESS_SERVICE))
    set(CC_GAME_ACTIVITY_SOURCES
        ${CMAKE_CURRENT_LIST_DIR}/android-gamesdk/GameActivity/game-activity/include/game-activity/GameActivity.cpp
        ${CMAKE_CURRENT_LIST_DIR}/android-gamesdk/GameActivity/game-activity/include/game-activity/system_utils.cpp
        ${CMAKE_CURRENT_LIST_DIR}/android-gamesdk/GameActivity/game-activity/include/game-activity/native_app_glue/android_native_app_glue.c
        ${CMAKE_CURRENT_LIST_DIR}/android-gamesdk/GameActivity/game-activity/include/game-text-input/gametextinput.cpp
    )

    include(${CMAKE_CURRENT_LIST_DIR}/android-gamesdk/GameController/CMakeLists.txt)
    list(APPEND CC_EXTERNAL_SOURCES
        ${CC_GAME_ACTIVITY_SOURCES}
    )
    include_directories(${CMAKE_CURRENT_LIST_DIR}/android-gamesdk/GameActivity/game-activity/include)
endif()

if(ANDROID OR OHOS OR NX)
  add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/pvmp3dec)
  add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/tremolo)

  list(APPEND CC_EXTERNAL_LIBS
    pvmp3dec
    vorbisidec
  )
elseif(APPLE)
  if(USE_SOCKET)
      include(${CMAKE_CURRENT_LIST_DIR}/SocketRocket/CMakeLists.txt)
  endif()
elseif(NX_WINDOWS)
  include(${CMAKE_CURRENT_LIST_DIR}/pvmp3dec/CMakeLists.txt)
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

# box2d
if(USE_BOX2D_JSB)
  add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/box2d/src)
  list(APPEND CC_EXTERNAL_LIBS box2d)
endif()