cmake_minimum_required( VERSION 2.8.5 )
project( rt-wdf )

add_definitions( -D_GNU_SOURCE )
SET(CMAKE_C_FLAGS  "${CMAKE_C_FLAGS} -std=gnu99")
SET(CMAKE_CXX_FLAGS  "${CMAKE_CXX_FLAGS} -D_GNU_SOURCE -std=gnu++0x -g -O3")
set(CMAKE_BUILD_TYPE Release)

include_directories(
  ${CMAKE_SOURCE_DIR}/
)

set(rt-wdf_SRCS 
    rt-wdf.cpp 
    rt-wdf_nlModels.cpp 
    rt-wdf_nlSolvers.cpp)
    
add_library(rt-wdf SHARED ${rt-wdf_SRCS})
set_property(TARGET rt-wdf PROPERTY VERSION "1.0.0")
set_property(TARGET rt-wdf PROPERTY SOVERSION 0)
install (TARGETS rt-wdf DESTINATION /usr/lib)
