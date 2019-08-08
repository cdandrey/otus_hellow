cmake_minimum_required(VERSION 3.2)

if (DEFINED ENV{TRAVIS_BUILD_NUMBER})
    message(STATUS TRAVIS_BUILD_NUMBER : $ENV{TRAVIS_BUILD_NUMBER})
    project(PrjGTest VERSION 0.0.$ENV{TRAVIS_BUILD_NUMBER})
else()
    project(PrjGTest VERSION 0.0.1)
endif()

find_package(GTest REQUIRED)
find_package(Threads REQUIRED)

add_executable(gtver gtest_version.cpp)

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
    ${GTEST_INCLUDE_DIRS}
)

target_link_libraries(gtver ${GTEST_BOTH_LIBRARIES}) 
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

add_test(gtver gtest_version)
