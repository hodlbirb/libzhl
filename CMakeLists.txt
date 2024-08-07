cmake_minimum_required(VERSION 3.5...3.30)
project(libsat VERSION 0.1.0)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

set(CMAKE_BUILD_TYPE $ENV{CMAKE_BUILD_TYPE})
set(CMAKE_GENERATOR "Ninja")

set(BUILD_SHARED_LIBS OFF)
set(CMAKE_TRY_COMPILE_TARGET_TYPE "STATIC_LIBRARY")

# Enable cppcheck
find_program(CPPCHECK cppcheck)
if(CPPCHECK)
    list(APPEND CMAKE_CXX_CPPCHECK "cppcheck;--enable=warning,performance;--inconclusive;--inline-suppr;--quiet")
else()
    message(SEND_ERROR "cppcheck not found, cannot run static code analysis")
endif()

set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
include(CPack)

include(CTest)
enable_testing()

find_program(CCACHE_FOUND ccache)
if(CCACHE_FOUND)
    set_property(GLOBAL PROPERTY RULE_LAUNCH_COMPILE "${CCACHE_FOUND}")
endif()

add_subdirectory(src)
add_subdirectory(tests)