#btest

cmake_minimum_required(VERSION 3.2)

if(DEFINED ENV{VCPKG_ROOT} AND NOT DEFINED CMAKE_TOOLCHAIN_FILE)
    set(CMAKE_TOOLCHAIN_FILE "$ENV{VCPKG_ROOT}/scripts/buildsystems/vcpkg.cmake" CACHE STRING "")
endif()

if (DEFINED ENV{TRAVIS_BUILD_NUMBER})
    project(PrjBTest VERSION 0.0.$ENV{TRAVIS_BUILD_NUMBER})
else()
    project(PrjBTest VERSION 0.0.1)
endif()

find_package(Boost COMPONENTS unit_test_framework REQUIRED)

add_executable(tver test_version.cpp)

set_target_properties(
    tver
    PROPERTIES
    CXX_STANDARD 14
    CXX_STANDARD_REQUIRED ON
)

target_include_directories(
    tver
    PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/../lib
    ${Boost_INCLUDE_DIR}
)

target_link_libraries(tver libver)
target_link_libraries(tver ${Boost_LIBRARIES})

set_target_properties(
    tver
    PROPERTIES
    COMPILE_DEFINITIONS BOOST_TEST_DYN_LINK
)

if (MSVC)
    target_compile_options(
        tver
        PRIVATE
        /W4
    )
else()
    target_compile_options(
        tver
        PRIVATE
        -Wall -Wextra -pedantic -Werror
    )
endif()

enable_testing()

add_test(tver test_version)