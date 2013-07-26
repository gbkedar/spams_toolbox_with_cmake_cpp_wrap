cmake_minimum_required(VERSION 2.8.10)

###################################
# set project's name
###################################
project( Spams-toolbox C CXX )

####################################################################
# Make sure that binaries aren't being built in the source directory
####################################################################
if("${Spams-toolbox_BINARY_DIR}" MATCHES "^${Spams-toolbox_SOURCE_DIR}$")
  message(FATAL_ERROR "SPAMS toolbox must be built with separate source and binary directories.")
endif()

#########################################
# ADD NEW MODULES HERE
#########################################
option( BUILD_OPENMP "Build with OpenMP support" ON )

################################################################################
# COMPILER FLAGS FOR PARALLELIZATION AND OTHERS 
#
################################################################################
if(BUILD_OPENMP)
  if(MSVC)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} /openmp")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /openmp")
  elseif(CMAKE_COMPILER_IS_GNUCC OR CMAKE_COMPILER_IS_GNUCXX)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fopenmp")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fopenmp")
  endif (MSVC)
endif(BUILD_OPENMP)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -largeArrayDims")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -largeArrayDims")

#########################################
# ADD SUBDIRS HERE
#########################################
#add_subdirectory( dictLearn )
#add_subdirectory( dag       )
add_subdirectory( prox/mex   )
#add_subdirectory( linalg    )
#add_subdirectory( decomp    )

#####################################################################################
# SETUP FOR BUILDING ADD INSTALLERS HERE
######################################################################################
set( EXE_DIR ${Spams-toolbox_BINARY_DIR}/exe CACHE PATH
  "Single Directory for all executables")
set( LIB_DIR ${Spams-toolbox_BINARY_DIR}/libs CACHE PATH
  "Single Directory for all static Libraries")
set( CMAKE_RUNTIME_OUTPUT_DIRECTORY ${EXE_DIR} )
set( CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${LIB_DIR} )

################################################################################
# Spams-toolbox DEPENDS ON EXTERNAL LIBRARIES
# THESE ARE BLAS and LAPACK
# WE MAKE SURE THEY ARE PRESENT HERE
################################################################################
#Find LAPACK - calls find BLAS within
find_package(BLAS REQUIRED)

