#lib

cmake_minimum_required(VERSION 3.2)

if (DEFINED ENV{TRAVIS_BUILD_NUMBER})
    project(PrjLibrary VERSION 0.0.$ENV{TRAVIS_BUILD_NUMBER})
else()
    project(PrjLibrary VERSION 0.0.1)
endif()

configure_file(version.h.in ${CMAKE_CURRENT_BINARY_DIR}/version.h)
add_library(libver STATIC lib.cpp lib.h)

set_target_properties(
    libver
    PROPERTIES
    CXX_STANDARD 14
    CXX_STANDARD_REQUIRED ON
)

target_include_directories(
    libver
    PRIVATE 
    ${CMAKE_CURRENT_BINARY_DIR}
)

if (MSVC)
    target_compile_options(
        libver
        PRIVATE
        /W4
    )
else ()
    target_compile_options(
        libver
        PRIVATE
        -Wall -Wextra -pedantic -Werror
    )
endif()
