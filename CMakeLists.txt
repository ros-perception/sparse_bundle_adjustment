cmake_minimum_required(VERSION 3.1)
project(sparse_bundle_adjustment)

if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 11)
endif()

find_package(catkin REQUIRED)
find_package(Eigen3 REQUIRED)

include_directories(${catkin_INCLUDE_DIRS} ${EIGEN3_INCLUDE_DIRS})
add_definitions(${EIGEN3_DEFINITIONS})

INCLUDE_DIRECTORIES(include)

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES sba
)

add_definitions(-DSBA_CHOLMOD)

# SBA library
add_library(sba src/sba.cpp src/spa.cpp src/spa2d.cpp src/csparse.cpp src/proj.cpp src/node.cpp src/sba_file_io.cpp)
#rosbuild_add_compile_flags(sba ${SSE_FLAGS})
target_link_libraries(sba blas lapack cholmod cxsparse)

install(DIRECTORY include/sparse_bundle_adjustment/
        DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION})

install(TARGETS sba
        LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION})
