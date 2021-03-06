cmake_minimum_required(VERSION 2.8)
project(edlib)

if(CMAKE_BUILD_TYPE MATCHES Debug)
  message("Debug mode")
  if(CMAKE_COMPILER_IS_GNUCC OR CMAKE_COMPILER_IS_GNUCXX)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra -pedantic")
  endif()
endif()

# Define output directories for created binaries and libraries.
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

include_directories(edlib/include)
file(GLOB SOURCES "edlib/src/*.cpp")

# Build binaries.
add_executable(helloWorld apps/hello-world/helloWorld.c ${SOURCES})
add_executable(runTests test/runTests.cpp ${SOURCES})
if (NOT WIN32) # If on windows, do not build binaries that do not support windows.
  add_executable(edlib-aligner apps/aligner/aligner.cpp ${SOURCES})
endif()

# Create libraries.
add_library(edlib SHARED ${SOURCES})
add_library(edlib_static STATIC ${SOURCES})

# Create target 'install' for installing libraries.
install(TARGETS edlib edlib_static DESTINATION lib)
install(FILES edlib/include/edlib.h DESTINATION include)
