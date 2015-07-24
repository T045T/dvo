cmake_minimum_required(VERSION 2.8.3)
project(dvo_core)

# Set the build type.  Options are:
#  Coverage       : w/ debug symbols, w/o optimization, w/ code-coverage
#  Debug          : w/ debug symbols, w/o optimization
#  Release        : w/o debug symbols, w/ optimization
#  RelWithDebInfo : w/ debug symbols, w/ optimization
#  MinSizeRel     : w/o debug symbols, w/ optimization, stripped binaries
#set(ROS_BUILD_TYPE RelWithDebInfo)

#set the default path for built executables to the "bin" directory
#set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set the default path for built libraries to the "lib" directory
#set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

#common commands for building c++ executables and libraries
#rosbuild_add_library(${PROJECT_NAME} src/example.cpp)
#target_link_libraries(${PROJECT_NAME} another_library)
#rosbuild_add_boost_directories()
#rosbuild_link_boost(${PROJECT_NAME} thread)
#rosbuild_add_executable(example examples/example.cpp)
#target_link_libraries(example ${PROJECT_NAME})

find_package(catkin REQUIRED COMPONENTS
  cmake_modules
  sophus
  )
# Eigen3 library
find_package(Eigen REQUIRED)

# VTK library
find_package(VTK REQUIRED)
find_package(PCL REQUIRED)
find_package(TBB REQUIRED)
find_package(Boost REQUIRED COMPONENTS thread)
find_package(OpenCV REQUIRED)

include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${PCL_INCLUDE_DIRS}
  ${Boost_INCLUDE_DIRS}
  ${Eigen_INCLUDE_DIRS}
  ${VTK_INCLUDE_DIRS}
  ${TBB_INCLUDE_DIRS}
  ${OpenCV_INCLUDE_DIRS}
  )

catkin_package(
  INCLUDE_DIRS include
  DEPENDS PCL
  CATKIN_DEPENDS sophus
  LIBRARIES ${PROJECT_NAME}
  )

add_definitions("-msse3")
add_definitions("-mfpmath=sse")

add_library(${PROJECT_NAME}
  src/core/interpolation.cpp
  src/core/intrinsic_matrix.cpp
  src/core/least_squares.cpp
  src/core/math_sse.cpp
  src/core/rgbd_image.cpp
  src/core/rgbd_image_sse.cpp
  src/core/surface_pyramid.cpp
  src/core/weight_calculation.cpp

  src/util/histogram.cpp

  src/visualization/async_point_cloud_builder.cpp
  src/visualization/point_cloud_aggregator.cpp
  src/visualization/visualizer.cpp
  src/visualization/camera_trajectory_visualizer.cpp
  src/visualization/pcl_camera_trajetory_visualizer.cpp

  src/dense_tracking.cpp
  src/dense_tracking_config.cpp
  )

target_link_libraries(${PROJECT_NAME}
  ${catkin_LIBRARIES}
  ${Boost_LIBRARIES}
  ${TBB_LIBRARIES}
  ${VTK_LIBRARIES}
  ${PCL_LIBRARIES}
  ${OpenCV_LIBRARIES}
  )

