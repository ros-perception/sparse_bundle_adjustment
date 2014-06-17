cmake_minimum_required(VERSION 2.8.3)
project(sparse_bundle_adjustment)

find_package(catkin REQUIRED
  COMPONENTS
    cmake_modules
)
find_package(Eigen REQUIRED)

include_directories(${catkin_INCLUDE_DIRS} ${EIGEN_INCLUDE_DIRS})
add_definitions(${EIGEN_DEFINITIONS})

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
