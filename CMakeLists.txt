cmake_minimum_required(VERSION 2.8.5 FATAL_ERROR)
project(Sherman)

# set (CMAKE_C_COMPILER "/usr/local/openmpi/bin/mpicxx")
# set (CMAKE_CXX_COMPILER ${CMAKE_C_COMPILER})

#Compiler options
# set(CMAKE_C_FLAGS "-Wall -Wno-deprecated-declarations -Wsign-compare -DNDEBUG -g")
set(CMAKE_C_FLAGS "-Wall -Wno-deprecated-declarations -Wsign-compare -O3 -g")
set(CMAKE_CXX_FLAGS "${CMAKE_C_FLAGS} -std=c++11 ")

# Link Options
set(LINKS_FLAGS "-lcityhash -lboost_system -lboost_coroutine -lpthread -libverbs -lmemcached")

# option (SANITIZE "Turn on sanitization" ON)
# if (SANITIZE)
    # set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fsanitize=address")
# endif()

set(INCLUDE_BASE ${PROJECT_SOURCE_DIR}/include)
include_directories(${INCLUDE_BASE})

#Source file define
set(COMMON_SRC ${PROJECT_SOURCE_DIR}/src)

#Used by both server and clients
file(GLOB_RECURSE COMMON_FILE ${COMMON_SRC}/*.cpp)
add_library(sherman STATIC ${COMMON_FILE})
link_libraries(sherman)

# test

file(GLOB TEST_SRC ${PROJECT_SOURCE_DIR}/test/*.cpp)
foreach (TEST ${TEST_SRC})
    get_filename_component(TEST_NAME ${TEST} NAME_WE)
    add_executable(${TEST_NAME} ${TEST})
    target_link_libraries(${TEST_NAME} ${LINKS_FLAGS})
endforeach()

