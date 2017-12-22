cmake_minimum_required(VERSION 3.4.0 FATAL_ERROR)


set(PROJECT_CONTACT romange@gmail.com)

enable_testing()

set(CMAKE_EXPORT_COMPILE_COMMANDS 1)

# Set targets in folders
set_property(GLOBAL PROPERTY USE_FOLDERS ON)

project(example CXX)


# Check target architecture
if (NOT CMAKE_SIZEOF_VOID_P EQUAL 8)
  message(FATAL_ERROR "Gaia requires a 64bit target architecture.")
endif()

if(NOT "${CMAKE_SYSTEM_NAME}" STREQUAL "Linux")
  message(FATAL_ERROR "Requires running on linux, found ${CMAKE_SYSTEM_NAME} instead")
endif()

find_package(Threads REQUIRED)
find_library (UNWIND_LIBRARY NAMES unwind DOC "unwind library")
mark_as_advanced (UNWIND_LIBRARY)  ## Hides this variable from GUI.

if (NOT UNWIND_LIBRARY)
  Message(FATAL_ERROR  "libunwind8-dev is not installed but advised for better glog stacktraces")
endif ()

set(CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/gaia/cmake" ${CMAKE_MODULE_PATH})

include(third_party)
include(internal)

include_directories(${CMAKE_CURRENT_SOURCE_DIR})
include_directories(gaia)

add_subdirectory(gaia)
add_subdirectory(examples)
