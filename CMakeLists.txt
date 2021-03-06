cmake_minimum_required(VERSION 2.8.12)
include(CheckIncludeFiles)

project(HPCsim)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O2 -g -W -Wall -Wextra -pedantic")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -O2 -g -W -Wall -Wextra -pedantic")
if ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -frounding-math -fsignaling-nans")
elseif ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "Intel")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fp-model precise -fp-model source -fimf-precision=high")
endif()
if ("${CMAKE_C_COMPILER_ID}" STREQUAL "GNU")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -frounding-math -fsignaling-nans")
elseif ("${CMAKE_C_COMPILER_ID}" STREQUAL "Intel")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fp-model precise -fp-model source -fimf-precision=high")
endif()

option(RESULT_FIELD_SIZE 0x800)
add_definitions(-DRESULT_FIELD_SIZE=${RESULT_FIELD_SIZE})

check_include_files(stdint.h HAVE_STDINT_H)
if (HAVE_STDINT_H)
    add_definitions(-DHAVE_STDINT_H)
endif()
find_package(Threads REQUIRED)

if(NOT DEFINED USE_PILOT_THREAD)
    set(USE_PILOT_THREAD FALSE)
endif()

if (USE_PILOT_THREAD)
    message("-- Using pilot thread jobs")
    add_definitions(-DUSE_PILOT_THREAD)
endif()

include_directories(SDK)
add_subdirectory(HPCsim)
add_subdirectory(examples)
