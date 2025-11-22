project(BULLET_PHYSICS)

set(BULLET_VERSION 3.27 CACHE STRING "Bullet3 version")

include_directories(${CMAKE_CURRENT_LIST_DIR})

subdirs(BulletInverseDynamics BulletSoftBody BulletCollision BulletDynamics LinearMath Bullet3Common)

add_library(ext_bullet INTERFACE)

target_include_directories(ext_bullet INTERFACE ${CMAKE_CURRENT_LIST_DIR})

target_link_libraries(ext_bullet INTERFACE
  BulletInverseDynamics
  BulletSoftBody
  BulletCollision
  BulletDynamics
  LinearMath
  Bullet3Common
)