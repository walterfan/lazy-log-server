cmake_minimum_required(VERSION 3.10)

project(lazy-log-server C CXX)

enable_testing()

if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE "Debug")
endif()

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/bin)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++17 -Wall -Ofast -Wfatal-errors -g  -D_MWAITXINTRIN_H_INCLUDED")

if (ENABLE_TEST_COVERAGE)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fprofile-arcs -ftest-coverage")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fprofile-arcs -ftest-coverage")
endif()


if(CMAKE_BUILD_BITS EQUAL 32)
  list(APPEND CXX_FLAGS "-m32")
endif()

string(REPLACE ";" " " CMAKE_CXX_FLAGS "${CXX_FLAGS}")

if(UNIX AND NOT APPLE)
  set(CMAKE_CXX_COMPILER "g++-10")
endif()

set(CMAKE_CXX_FLAGS_DEBUG "-O0")
set(CMAKE_CXX_FLAGS_RELEASE "-O2 -DNDEBUG")

set(EXECUTABLE_OUTPUT_PATH ${CMAKE_SOURCE_DIR}/bin)
set(LIBRARY_OUTPUT_PATH ${CMAKE_SOURCE_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/bin)

SET(Boost_USE_STATIC_LIBS ON)
SET(Boost_USE_MULTITHREAD ON)

#add_compile_definitions(BOOST_LOG_DYN_LIN)
#ADD_DEFINITIONS(-DBOOST_LOG_DYN_LINK)

find_package(Boost REQUIRED COMPONENTS system thread regex log log_setup program_options asio beast)

find_package(GTest)

include_directories(${Boost_INCLUDE_DIRS})
include_directories(${PROJECT_SOURCE_DIR})
include_directories(${PROJECT_SOURCE_DIR}/util)
include_directories(${PROJECT_SOURCE_DIR}/src)

link_directories(/usr/local/lib)

string(TOUPPER ${CMAKE_BUILD_TYPE} BUILD_TYPE)
message(STATUS "CXX_FLAGS = " ${CMAKE_CXX_FLAGS} " " ${CMAKE_CXX_FLAGS_${BUILD_TYPE}})


if (APPLE)
    set(GTEST_INCLUDE_DIRS /opt/homebrew/Cellar/googletest/1.14.0/include)
    set(GTEST_LIBRARY_DIRS /opt/homebrew/Cellar/googletest/1.14.0/lib)
    include_directories(${GTEST_INCLUDE_DIRS})
    link_directories(${GTEST_LIBRARY_DIRS})

endif()

add_subdirectory(src)




