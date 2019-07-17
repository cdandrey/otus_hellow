#src

cmake_minimum_required(VERSION 3.2)

if (DEFINED ENV{TRAVIS_BUILD_NUMBER})
    project(PrjMain VERSION 0.0.$ENV{TRAVIS_BUILD_NUMBER})
else()
    project(PrjMain VERSION 0.0.1)
endif()

add_executable(ver main.cpp)

set_target_properties(
    ver
    PROPERTIES
    CXX_STANDARD 14
    CXX_STANDARD_REQUIRED ON
)

target_include_directories(
    ver
    PRIVATE 
    ${CMAKE_CURRENT_SOURCE_DIR}/../lib
)

target_link_libraries(ver libver)

if (MSVC)

    target_compile_options(
        ver
        PRIVATE
        /W4
    )

else ()

    target_compile_options(
        ver
        PRIVATE
        -Wall -Wextra -pedantic -Werror
    )

endif()

install(TARGETS ver RUNTIME DESTINATION bin)



