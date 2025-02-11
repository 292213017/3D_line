cmake_minimum_required(VERSION 2.6 FATAL_ERROR)
project(PCL_test)

# set(CMAKE_CXX_FLAGS_DEBUG "$ENV{CXXFLAGS} -O0 -Wall -g -ggdb")
# set(CMAKE_CXX_FLAGS_RELEASE "$ENV{CXXFLAGS} -O3 -Wall")
# ADD_EXECUTABLE(main ${DIR_SRCS})

# OpenMP
FIND_PACKAGE(OpenMP)
IF(OPENMP_FOUND)
	OPTION(WITH_OPENMP "Whether to use parallel processing capabilities of OPENMP. ON/OFF" ON)
ENDIF(OPENMP_FOUND)

IF(OPENMP_FOUND AND WITH_OPENMP)
	MESSAGE(STATUS "With OpenMP ")
	SET (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DMAX_OPENMP_NUM_THREADS=${PROCESSOR_COUNT} -DOPENMP_NUM_THREADS=${PROCESSOR_COUNT} ${OpenMP_CXX_FLAGS} -DOPENMP")
	SET(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -DMAX_OPENMP_NUM_THREADS=${PROCESSOR_COUNT} -DOPENMP_NUM_THREADS=${PROCESSOR_COUNT} ${OpenMP_CXX_FLAGS} ${OpenMP_C_FLAGS} -DOPENMP")
ELSE(OPENMP_FOUND AND WITH_OPENMP)
	MESSAGE(STATUS "Without OpenMP")
	SET (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DMAX_OPENMP_NUM_THREADS=1 -DOPENMP_NUM_THREADS=1")
	SET (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -DMAX_OPENMP_NUM_THREADS=1 -DOPENMP_NUM_THREADS=1")
ENDIF(OPENMP_FOUND AND WITH_OPENMP)

# OpenCV
#SET(OpenCV_DIR "E:/installer/opencv/build")
FIND_PACKAGE(OpenCV REQUIRED)

find_package(PCL REQUIRED)
include_directories(${PCL_INCLUDE_DIRS})
link_directories(${PCL_LIBRARY_DIRS})
add_definitions(${PCL_DEFINITIONS})
add_executable(pcd_write_test pcd_write.cpp)
target_link_libraries(pcd_write_test ${PCL_LIBRARIES})
add_executable(pcd_nor_test point_nor.cpp)
target_link_libraries(pcd_nor_test ${PCL_LIBRARIES} ${OpenCV_LIBS})
add_executable(line_3D line_3D.cpp)
add_executable( joinMap joinMap.cpp )
target_link_libraries( joinMap ${OpenCV_LIBS} ${PCL_LIBRARIES} )