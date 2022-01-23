cmake_minimum_required(VERSION 3.16)

project(airkiss_monitor)

FILE(GLOB_RECURSE ALL_SOURCES
  "*.c"
)

FILE(GLOB_RECURSE ALL_INCLUDES 
  "*.h"
)

SET(ALL_SRCS
  ${ALL_SOURCES}
  ${ALL_INCLUDES}
)

source_group(TREE ${CMAKE_SOURCE_DIR} FILES ${ALL_SRCS})

include_directories(
  ${CMAKE_SOURCE_DIR}
)

MESSAGE(STATUS "operation system is ${CMAKE_SYSTEM}")

if(UNIX AND NOT APPLE)
  set(LINUX TRUE)
endif()

if(LINUX)
  find_library(IW_LIB iw)
  if(NOT IW_LIB)
    message(FATAL_ERROR "iw library not found")
  else()
    message(${IW_LIB})
  endif()
endif()

# aux_source_directory(. DIR_SRCS)
# aux_source_directory(utils DIR_SRCS)
# aux_source_directory(capture DIR_SRCS)
# aux_source_directory(capture/radiotap DIR_SRCS)

add_executable(airkiss_monitor
    ${ALL_SRCS}
)

if(LINUX)
  target_link_libraries(airkiss_monitor
    ${IW_LIB}
  )
endif()