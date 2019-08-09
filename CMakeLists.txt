#root

cmake_minimum_required(VERSION 3.2)

if(DEFINED ENV{VCPKG_ROOT} AND NOT DEFINED CMAKE_TOOLCHAIN_FILE)
    set(CMAKE_TOOLCHAIN_FILE "$ENV{VCPKG_ROOT}/scripts/buildsystems/vcpkg.cmake" CACHE STRING "")
endif()

if (DEFINED ENV{TRAVIS_BUILD_NUMBER})
    project(PrjHelloworld VERSION 0.0.$ENV{TRAVIS_BUILD_NUMBER})
else()
    project(PrjHelloworld VERSION 0.0.1)
endif()

set(CMAKE_VERBOSE_MAKEFILE ON)

add_subdirectory(src)
add_subdirectory(lib)
//add_subdirectory(btest)
add_subdirectory(gtest)

set(CPACK_GENERATOR DEB)

set(CPACK_PACKAGE_VERSION_MAJOR "${PROJECT_VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${PROJECT_VERSION_MINOR}")
set(CPACK_PACKAGE_VERSION_PATCH "${PROJECT_VERSION_PATCH}")

set(CPACK_PACKAGE_CONTACT cdandrey@gmail.com)

include(CPack)