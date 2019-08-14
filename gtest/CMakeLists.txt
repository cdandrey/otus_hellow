cmake_minimum_required(VERSION 3.2)

set (PRJ_NAME prj_gtver)
set (SOURCES gtest_version.cpp)

if (DEFINED ENV{TRAVIS_BUILD_NUMBER})
    message(STATUS TRAVIS_BUILD_NUMBER : $ENV{TRAVIS_BUILD_NUMBER})
    project(${PRJ_NAME} VERSION 0.0.$ENV{TRAVIS_BUILD_NUMBER})
else()
    project(${PRJ_NAME} VERSION 0.0.1)
endif()

find_package(GTest REQUIRED)
find_package(Threads REQUIRED)

add_executable(gtver ${SOURCES})

include_directories( 
    ${GTES_INCLUDE_DIRS}
)

set_target_properties(
    gtver
    PROPERTIES
    CXX_STANDARD 14
    CXX_STANDARD_REQUIRED ON
)

target_include_directories(
    gtver
    PRIVATE 
    ${CMAKE_CURRENT_SOURCE_DIR}/../lib
)

target_link_libraries(gtver ${GTEST_BOTH_LIBRARIES}) 
target_link_libraries(gtver ${CMAKE_THREAD_LIBS_INIT})
target_link_libraries(gtver libver)

if (MSVC)
    target_compile_options(
        gtver
        PRIVATE
        /W4
    )
else ()
    target_compile_options(
        gtver
        PRIVATE
        -Wall -Wextra -pedantic -Werror
    )
endif()

enable_testing()

add_test(gtver gtver)
