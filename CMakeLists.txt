cmake_minimum_required (VERSION 3.1)
project (Propaga)

set (CMAKE_CXX_STANDARD 11)
set (MAJOR_VERSION 1)
set (MINOR_VERSION 0)
set (FIX_RELEASE 3)

configure_file (
  "${PROJECT_SOURCE_DIR}/src/version.h.in"
  "${PROJECT_BINARY_DIR}/version.h"
  )

add_definitions(-D_SCL_SECURE_NO_WARNINGS)
add_definitions(-D_CRT_SECURE_NO_WARNINGS)
add_definitions(-D_USE_MATH_DEFINES)

file(GLOB Propaga_SOURCES "src/*.cpp")
file(GLOB Propaga_HEADERS "src/*.h")
set (Propaga_INCLUDE_DIRS "src/jsoncons/src/")
add_executable (Propaga ${Propaga_SOURCES})

# Require MPI for this project
find_package(MPI REQUIRED)
set(CMAKE_CXX_COMPILE_FLAGS ${CMAKE_CXX_COMPILE_FLAGS} ${MPI_COMPILE_FLAGS})
set(CMAKE_CXX_LINK_FLAGS ${CMAKE_CXX_LINK_FLAGS} ${MPI_LINK_FLAGS})
include_directories(${MPI_INCLUDE_PATH} ${Propaga_INCLUDE_DIRS} ${PROJECT_BINARY_DIR})
target_link_libraries(Propaga ${MPI_LIBRARIES}) 
install (TARGETS Propaga DESTINATION ${CMAKE_SOURCE_DIR}/bin/)

# Search for OpenMP and use it
find_package(OpenMP)
if (OPENMP_FOUND)
    set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
    set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
else()
    add_definitions(-DNO_OMP)
endif()

