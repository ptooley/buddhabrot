cmake_minimum_required(VERSION 2.8)
 
PROJECT(mandel)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED on)
 
if( NOT CMAKE_BUILD_TYPE )
  set( CMAKE_BUILD_TYPE Debug CACHE STRING
             "Choose the type of build, options are: None Debug Release"
        FORCE )
endif()

set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -Wall -O0 -ggdb -march=native")
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -Wall -O3 -march=native")

find_package(VTK REQUIRED)
include(${VTK_USE_FILE})
set(EXTRA_LIBS ${EXTRA_LIBS} ${VTK_LIBRARIES})
 
find_package(HDF5 REQUIRED COMPONENTS C HL)
include_directories(${HDF5_INCLUDE_DIRS})  
set(EXTRA_LIBS ${EXTRA_LIBS} ${HDF5_C_LIBRARIES} ${HDF5_HL_LIBRARIES})

set (MANDELSOURCES
  main
  trajectorygenerator
)

set (TESTSOURCES
  vtktest
  imageanimator
  cuehandler
  trajectorygenerator
)

add_executable(mandel ${MANDELSOURCES})
set_target_properties(mandel PROPERTIES LINKER_LANGUAGE CXX)
target_link_libraries(mandel ${EXTRA_LIBS})

add_executable(vtktest ${TESTSOURCES})
set_target_properties(vtktest PROPERTIES LINKER_LANGUAGE CXX)
target_link_libraries(vtktest ${EXTRA_LIBS})