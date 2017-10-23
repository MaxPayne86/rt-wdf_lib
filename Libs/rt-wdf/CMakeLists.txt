cmake_minimum_required( VERSION 2.8.5 )
project( rt-wdf )

set(INSTALL_LIB_DIR "${CMAKE_INSTALL_PREFIX}/lib" CACHE PATH "Installation directory for libraries.")
set(INSTALL_INC_DIR "${CMAKE_INSTALL_PREFIX}/include" CACHE PATH "Installation directory for headers.")
set(INSTALL_PKGCONFIG_DIR "${CMAKE_INSTALL_PREFIX}/share/pkgconfig" CACHE PATH "Installation directory for pkgconfig file.")

# This is the ABI version number, which may differ from the
# API version number (defined in rt-wdf.h).
# Be sure to also update these in Makefile and MANIFEST!
set(SO_MAJOR 2)
set(SO_MINOR 1)
set(SO_PATCH 0)

set(CMAKE_CXX_FLAGS  "${CMAKE_CXX_FLAGS} -D_GNU_SOURCE -std=gnu++0x -g -O3")
set(CMAKE_BUILD_TYPE Release)

set(rt-wdf_SRCS 
    rt-wdf.cpp 
    rt-wdf_nlModels.cpp 
    rt-wdf_nlSolvers.cpp
    rt-wdf.h
    rt-wdf_nlModels.h
    rt-wdf_nlSolvers.h
    rt-wdf_types.h)
    
add_library(rt-wdf SHARED ${rt-wdf_SRCS})

set(RTWDF_FULL_VERSION "${SO_MAJOR}.${SO_MINOR}.${SO_PATCH}")

set_target_properties (rt-wdf PROPERTIES
  POSITION_INDEPENDENT_CODE ON
  VERSION "${RTWDF_FULL_VERSION}"
  SOVERSION ${SO_MAJOR}
)

configure_file(${CMAKE_CURRENT_SOURCE_DIR}/rt-wdf.pc.cmakein
               ${CMAKE_CURRENT_SOURCE_DIR}/rt-wdf.pc @ONLY)
configure_file(${CMAKE_CURRENT_SOURCE_DIR}/${PROJECT_NAME}-config.cmake.cmakein
               ${CMAKE_CURRENT_SOURCE_DIR}/${PROJECT_NAME}-config.cmake @ONLY)
configure_file(${CMAKE_CURRENT_SOURCE_DIR}/${PROJECT_NAME}-config-version.cmake.cmakein
               ${CMAKE_CURRENT_SOURCE_DIR}/${PROJECT_NAME}-config-version.cmake @ONLY)

install(FILES rt-wdf.pc DESTINATION "${INSTALL_PKGCONFIG_DIR}")
install(FILES rt-wdf.h DESTINATION "${INSTALL_INC_DIR}")
install(TARGETS rt-wdf
        ARCHIVE DESTINATION "${INSTALL_LIB_DIR}"
        LIBRARY DESTINATION "${INSTALL_LIB_DIR}")
  
install(FILES ${PROJECT_NAME}-config.cmake ${PROJECT_NAME}-config-version.cmake
DESTINATION "${INSTALL_LIB_DIR}/${PROJECT_NAME}")