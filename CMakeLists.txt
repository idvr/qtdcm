cmake_minimum_required(VERSION 2.6)

project(QTDCM)

set(${PROJECT_NAME}_VERSION_MAJOR 2 CACHE STRING "${PROJECT_NAME} major version number.")
set(${PROJECT_NAME}_VERSION_MINOR 0 CACHE STRING "${PROJECT_NAME} minor version number.")
set(${PROJECT_NAME}_VERSION_PATCH 2 CACHE STRING "${PROJECT_NAME} patch version number.")
set(${PROJECT_NAME}_VERSION "${${PROJECT_NAME}_VERSION_MAJOR}.${${PROJECT_NAME}_VERSION_MINOR}.${${PROJECT_NAME}_VERSION_PATCH}")

mark_as_advanced(${PROJECT_NAME}_VERSION_MAJOR)
mark_as_advanced(${PROJECT_NAME}_VERSION_MINOR)
mark_as_advanced(${PROJECT_NAME}_VERSION_PATCH)

option(BUILD_EXAMPLE "Build qtdcm example application" OFF)
option(BUILD_SHARED_LIBS "Build QtDcm with shared libraries." OFF)
option(BUILD_DOCUMENTATION "Build QtDcm Documentation (add a Documentation target)" OFF)
option(BUILD_PACKAGE "Configure QtDcm packaging" OFF)

option(BUILD_SHARED_LIBS "Build Qtdcm with shared libraries." ON)

if(BUILD_PACKAGE)
  include (${QTDCM_SOURCE_DIR}/CMake/CPackConfig.cmake)
endif(BUILD_PACKAGE)

set(EXECUTABLE_OUTPUT_PATH "${QTDCM_BINARY_DIR}/bin")
if (WIN32 AND BUILD_SHARED_LIBS)
  set(LIBRARY_OUTPUT_PATH "${QTDCM_BINARY_DIR}/bin")
else (WIN32 AND BUILD_SHARED_LIBS)
  set(LIBRARY_OUTPUT_PATH "${QTDCM_BINARY_DIR}/lib")
endif (WIN32 AND BUILD_SHARED_LIBS)
mark_as_advanced(LIBRARY_OUTPUT_PATH EXECUTABLE_OUTPUT_PATH)

#Configure doxygen documentation
if(BUILD_DOCUMENTATION)
  add_subdirectory(Documentation/Doxygen)
endif(BUILD_DOCUMENTATION)

set (QTDCM_CMAKE_MODULE_PATH ${QTDCM_SOURCE_DIR}/CMake)
set (CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${QTDCM_CMAKE_MODULE_PATH}")

add_subdirectory(Code)

if (BUILD_EXAMPLE)
  add_subdirectory(Example)
endif()

## ###################################################################
## INSTALL RULES
## ###################################################################
if( EXISTS ${${PROJECT_NAME}_BINARY_DIR}/QtDCMConfig.cmake
AND EXISTS ${${PROJECT_NAME}_BINARY_DIR}/UseQtDCM.cmake)

install(FILES
  ${${PROJECT_NAME}_BINARY_DIR}/QtDCMConfig.cmake
  ${${PROJECT_NAME}_BINARY_DIR}/UseQtDCM.cmake
  DESTINATION
  share/qtdcm)

endif( EXISTS ${${PROJECT_NAME}_BINARY_DIR}/QtDCMConfig.cmake
   AND EXISTS ${${PROJECT_NAME}_BINARY_DIR}/UseQtDCM.cmake)
