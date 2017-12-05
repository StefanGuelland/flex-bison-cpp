cmake_minimum_required(VERSION 3.9)
project(flex_bison)

set(CMAKE_CXX_STANDARD 17)

# Problem aus Flatpak
SET(GCC_COVERAGE_LINK_FLAGS "-Wl,--dynamic-linker=/lib64/ld-linux-x86-64.so.2")
SET(CMAKE_EXE_LINKER_FLAGS  "${CMAKE_EXE_LINKER_FLAGS} ${GCC_COVERAGE_LINK_FLAGS}" )

#https://cmake.org/cmake/help/v3.0/module/FindFLEX.html
find_package(BISON)
find_package(FLEX)

BISON_TARGET(MyParser parser.yy ${CMAKE_CURRENT_BINARY_DIR}/parser.cc
        COMPILE_FLAGS "--language=c++")
FLEX_TARGET(MyScanner scanner.ll  ${CMAKE_CURRENT_BINARY_DIR}/scanner.cc
        COMPILE_FLAGS "--c++")
ADD_FLEX_BISON_DEPENDENCY(MyScanner MyParser)


include_directories(${CMAKE_CURRENT_BINARY_DIR})
include_directories(${CMAKE_CURRENT_SOURCE_DIR})
add_executable(flex_bison
        ${BISON_MyParser_OUTPUTS}
        ${FLEX_MyScanner_OUTPUTS}
        driver.cc
        driver.h
        scanner.h
        y.tab.h
        expression.h
        exprtest.cc
        )