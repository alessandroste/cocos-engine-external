project(external)

add_library(external INTERFACE)

set(CC_EXTERNAL_LIBS)
set(CC_EXTERNAL_INCLUDES ${CMAKE_CURRENT_LIST_DIR}/sources)

# CPM managed sources
set(CPM_SOURCE_CACHE ${CMAKE_CURRENT_LIST_DIR}/packages)
include(cmake/CPM.cmake)

CPMAddPackage(
    NAME libwebsockets
    GIT_REPOSITORY https://libwebsockets.org/repo/libwebsockets.git
    GIT_TAG v4.4-stable
    OPTIONS
        "LWS_WITH_SHARED OFF"
        "LWS_WITHOUT_SERVER ON"
        "LWS_WITH_MINIMAL_EXAMPLES OFF"
        "LWS_WITHOUT_TESTAPPS ON"
        "LWS_WITHOUT_TEST_SERVER ON"
        "LWS_WITHOUT_TEST_CLIENT ON"
        "LWS_WITHOUT_TEST_PING ON"
)

list(APPEND CC_EXTERNAL_LIBS
    websockets
)

if (WINDOWS OR LINUX)
    list(APPEND CC_EXTERNAL_LIBS
        glfw
        # OpenAL::OpenAL
    )

    CPMAddPackage(
        NAME glfw3
        GITHUB_REPOSITORY glfw/glfw
        GIT_TAG 3.4
        OPTIONS
            "GLFW_BUILD_TESTS OFF"
            "GLFW_BUILD_EXAMPLES OFF"
            "GLFW_BUILD_DOCS OFF"
    )

    # CPMAddPackage(
    #     NAME openal
    #     GITHUB_REPOSITORY kcat/openal-soft
    #     GIT_TAG 1.24.3
    #     OPTIONS
    #         "LIBTYPE STATIC"
    #         "ALSOFT_UTILS OFF"
    #         "ALSOFT_EXAMPLES OFF"
    #         "ALSOFT_TESTS OFF"
    # )

    # add_compile_definitions(-DAL_LIBTYPE_STATIC)
endif()

MESSAGE(STATUS "platform path: ${platform_spec_path}")
include(${CMAKE_CURRENT_LIST_DIR}/cmake/CocosExternalConfig.cmake)
include(${CMAKE_CURRENT_LIST_DIR}/sources/CMakeLists.txt)
if(WINDOWS OR NX_WINDOWS)
    include(${CMAKE_CURRENT_LIST_DIR}/win64/CMakeLists.txt)
elseif(ANDROID)
    include(${CMAKE_CURRENT_LIST_DIR}/android/CMakeLists.txt)
elseif(APPLE)
    if(MACOSX)
        include(${CMAKE_CURRENT_LIST_DIR}/mac/CMakeLists.txt)
    elseif(IOS)
        # include(${CMAKE_CURRENT_LIST_DIR}/ios-m1-simulator/CMakeLists.txt) # Uncomment this line and comment next line if using M1 simulator
        include(${CMAKE_CURRENT_LIST_DIR}/ios/CMakeLists.txt)
    endif()
endif()

target_include_directories(external INTERFACE
  ${CC_EXTERNAL_INCLUDES}
)

target_link_libraries(external INTERFACE
  ${CC_EXTERNAL_LIBS}
)