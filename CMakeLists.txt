CMAKE_MINIMUM_REQUIRED(VERSION 2.8)
project(OpenVDB)

find_package(Boost REQUIRED)
find_package(PythonLibs REQUIRED)

set(DEFAULT_INCL_DIR "/usr/include")
set(DEFAULT_LIB_DIR "/usr/lib")

set(TBB_INCL_DIR ${DEFAULT_INCL_DIR} CACHE PATH "TBB include directory")
set(TBB_LIB_DIR ${DEFAULT_LIB_DIR} CACHE PATH "TBB library directory")

set(LOG4CPLUS_INCL_DIR ${DEFAULT_INCL_DIR} CACHE PATH "log4cpp include directory")
set(LOG4CPLUS_LIB_DIR ${DEFAULT_LIB_DIR} CACHE PATH "log4cpp library directory")

set(GLFW_INCL_DIR ${DEFAULT_INCL_DIR} CACHE PATH "GLFW include directory")
set(GLFW_LIB_DIR ${DEFAULT_LIB_DIR} CACHE PATH "GLFW library directory")
set(GLFW_MAJOR_VERSION 2 CACHE STRING "GLFW Major Version")

set(CPPUNIT_INCL_DIR ${DEFAULT_INCL_DIR} CACHE PATH "cppunit include directory")
set(CPPUNIT_LIB_DIR ${DEFAULT_LIB_DIR} CACHE PATH "cppunit library directory")

set(PYTHON_VERSION ${PYTHONLIBS_VERSION_STRING} CACHE STRING "Python version")
set(PYTHON_INCL_DIR ${PYTHON_INCLUDE_DIRS} CACHE PATH "Python include directory")
message("PYTHON_LIBRARY=" ${PYTHON_LIBRARY})
get_filename_component(PYTHON_LIB_DIR ${PYTHON_LIBRARY} DIRECTORY CACHE)
set(PYCONFIG_INCL_DIR ${PYTHON_INCL_DIR} CACHE PATH "Directory containing pyconfig.h")

set(NUMPY_INCL_DIR "/usr/lib/python2.7/dist-packages/numpy/core/include/numpy" CACHE PATH "Directory containing arrayobject.h")

set(OpenVDB_BUILD_DIR "${CMAKE_CURRENT_BINARY_DIR}/OpenVDB")
set(OpenVDB_SRC_DIR "${CMAKE_CURRENT_SOURCE_DIR}/OpenVDB/openvdb")
set(OpenVDB_BUILD_CMD make -C ${OpenVDB_SRC_DIR} DESTDIR=${OpenVDB_BUILD_DIR}
  BOOST_INCL_DIR=${Boost_INCLUDE_DIRS} BOOST_LIB_DIR=${Boost_LIBRARY_DIRS}
  TBB_INCL_DIR=${TBB_INCLUDE_DIR} TBB_LIB_DIR=${TBB_LIB_DIR}
  LOG4CPLUS_INCL_DIR=${LOG4CPLUS_INCLUDE_DIR} LOG4CPLUS_LIB_DIR=${LOG4CPLUS_LIB_DIR}
  PYTHON_VERSION=${PYTHON_VERSION} PYTHON_INCL_DIR=${PYTHON_INCL_DIR} PYTHON_LIB_DIR=${PYTHON_LIB_DIR} PYCONFIG_INCL_DIR=${PYCONFIG_INCL_DIR}
  GLFW_INCL_DIR=${GLFW_INCL_DIR} GLFW_LIB_DIR=${GLFW_LIB_DIR} GLFW_MAJOR_VERSION=${GLFW_MAJOR_VERSION}
  CPPUNIT_INCL_DIR=${CPPUNIT_INCL_DIR} CPPUNIT_LIB_DIR=${CPPUNIT_LIB_DIR}
  NUMPY_INCL_DIR=${NUMPY_INCL_DIR}
  )

message("OpenVDB_BUILD_CMD = ${OpenVDB_BUILD_CMD}")

include(ExternalProject)
ExternalProject_Add(OpenVDB
  PREFIX ${OpenVDB_BUILD_DIR}
  SOURCE_DIR ${OpenVDB_SRC_DIR}
  CONFIGURE_COMMAND ""
  BUILD_COMMAND ${OpenVDB_BUILD_CMD}
  INSTALL_COMMAND ""
  )