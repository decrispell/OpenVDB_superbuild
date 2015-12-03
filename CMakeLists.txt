CMAKE_MINIMUM_REQUIRED(VERSION 2.8)
project(OpenVDB)

find_package(Boost REQUIRED)
find_package(Boost COMPONENTS python)
find_package(PythonLibs REQUIRED)

set(DEFAULT_INCL_DIR "/usr/include")
set(DEFAULT_LIB_DIR "/usr/lib")

list(GET Boost_INCLUDE_DIRS 0 BOOST_INCL_DIR0)
set(BOOST_INCL_DIR ${BOOST_INCL_DIR0} CACHE PATH "Boost include directory for OpenVDB")
list(GET Boost_LIBRARY_DIRS 0 BOOST_LIB_DIR0)
set(BOOST_LIB_DIR ${BOOST_LIB_DIR0} CACHE PATH "Boost library directory for OpenVDB")

set(BOOST_PYTHON_LIB_DIR ${BOOST_LIB_DIR0} CACHE PATH "Directory containing boost-python library")
set(BOOST_PYTHON_LIB "-lboost_python" CACHE STRING "Linker flag for boost_python library")

set(TBB_INCL_DIR ${DEFAULT_INCL_DIR} CACHE PATH "TBB include directory")
set(TBB_LIB_DIR ${DEFAULT_LIB_DIR} CACHE PATH "TBB library directory")

set(ILMBASE_LIB_DIR "/usr/lib/x86_64-linux-gnu/" CACHE PATH "Directory containing libHalf library")
set(ILMBASE_INCL_DIR "${DEFAULT_INCL_DIR}/OpenEXR" CACHE PATH "Directory containing OpenEXR header files")

set(EXR_INCL_DIR ${DEFAULT_INCL_DIR} CACHE PATH "Directory containing EXR include dir")
set(EXR_LIB_DIR "/usr/lib/x86_64-linux-gnu/" CACHE PATH "Directory containing libIlmImf library")

set(CONCURRENT_MALLOC_LIB_DIR ${TBB_LIB_DIR} CACHE PATH "Directory containing scalable, concurrent malloc library (e.g. tbbmalloc)")
set(CONCURRENT_MALLOC_LIB "-ltbbmalloc" CACHE STRING "Linker flag for concurrent malloc library")

set(LOG4CPLUS_INCL_DIR ${DEFAULT_INCL_DIR} CACHE PATH "log4cplus include directory")
set(LOG4CPLUS_LIB_DIR ${DEFAULT_LIB_DIR} CACHE PATH "log4cplus library directory")

set(GLFW_INCL_DIR ${DEFAULT_INCL_DIR} CACHE PATH "GLFW include directory")
set(GLFW_LIB_DIR ${DEFAULT_LIB_DIR} CACHE PATH "GLFW library directory")
set(GLFW_MAJOR_VERSION 2 CACHE STRING "GLFW Major Version")

set(CPPUNIT_INCL_DIR ${DEFAULT_INCL_DIR} CACHE PATH "cppunit include directory")
set(CPPUNIT_LIB_DIR ${DEFAULT_LIB_DIR} CACHE PATH "cppunit library directory")

# TODO: extact major.minor from PYTHONLIBS_VERSION_STRING
set(PYTHON_VERSION "2.7" CACHE STRING "Python version for OpenVDB")
list(GET PYTHON_INCLUDE_DIRS 0 PYTHON_INCL_DIR0 ) 
set(PYTHON_INCL_DIR ${PYTHON_INCL_DIR0} CACHE PATH "Python include directory for OpenVDB")
message("PYTHON_LIBRARY=" ${PYTHON_LIBRARY})
get_filename_component(PYTHON_LIB_DIR ${PYTHON_LIBRARY} DIRECTORY CACHE)
set(PYCONFIG_INCL_DIR ${PYTHON_INCL_DIR} CACHE PATH "Directory containing pyconfig.h")

set(NUMPY_INCL_DIR "/usr/lib/python2.7/dist-packages/numpy/core/include/numpy" CACHE PATH "Directory containing arrayobject.h")

set(OpenVDB_INSTALL_DIR "${CMAKE_CURRENT_SRC_DIR}/../install" CACHE PATH "OpenVDB Install directory")
set(OpenVDB_SRC_DIR "${CMAKE_CURRENT_SOURCE_DIR}/OpenVDB/openvdb")
set(OpenVDB_BUILD_CMD make -C ${OpenVDB_SRC_DIR} install DESTDIR=${OpenVDB_INSTALL_DIR}
BOOST_INCL_DIR=${BOOST_INCL_DIR} BOOST_LIB_DIR=${BOOST_LIB_DIR}
BOOST_PYTHON_LIB_DIR=${BOOST_PYTHON_LIB_DIR} BOOST_PYTHON_LIB=${BOOST_PYTHON_LIB}
TBB_INCL_DIR=${TBB_INCL_DIR} TBB_LIB_DIR=${TBB_LIB_DIR}
CONCURRENT_MALLOC_LIB_DIR=${CONCURRENT_MALLOC_LIB_DIR} CONCURRENT_MALLOC_LIB=${CONCURRENT_MALLOC_LIB}
ILMBASE_INCL_DIR=${ILMBASE_INCL_DIR} ILMBASE_LIB_DIR=${ILMBASE_LIB_DIR}
EXR_INCL_DIR=${EXR_INCL_DIR} EXR_LIB_DIR=${EXR_LIB_DIR}
LOG4CPLUS_INCL_DIR=${LOG4CPLUS_INCL_DIR} LOG4CPLUS_LIB_DIR=${LOG4CPLUS_LIB_DIR}
PYTHON_VERSION=${PYTHON_VERSION} PYTHON_INCL_DIR=${PYTHON_INCL_DIR} PYTHON_LIB_DIR=${PYTHON_LIB_DIR} PYCONFIG_INCL_DIR=${PYCONFIG_INCL_DIR}
GLFW_INCL_DIR=${GLFW_INCL_DIR} GLFW_LIB_DIR=${GLFW_LIB_DIR} GLFW_MAJOR_VERSION=${GLFW_MAJOR_VERSION}
CPPUNIT_INCL_DIR=${CPPUNIT_INCL_DIR} CPPUNIT_LIB_DIR=${CPPUNIT_LIB_DIR}
NUMPY_INCL_DIR=${NUMPY_INCL_DIR}
)

message("OpenVDB_BUILD_CMD = ${OpenVDB_BUILD_CMD}")

include(ExternalProject)
ExternalProject_Add(OpenVDB
  SOURCE_DIR ${OpenVDB_SRC_DIR}
  CONFIGURE_COMMAND ""
  BUILD_COMMAND ${OpenVDB_BUILD_CMD}
  INSTALL_COMMAND ""
  )
