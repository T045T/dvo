cmake_minimum_required(VERSION 2.8.3)
project(dvo_ros)

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
  roscpp
  tf
  tf_conversions
  eigen_conversions
  pcl_ros
  sensor_msgs
  image_transport
  cv_bridge
  visualization_msgs
  interactive_markers
  message_filters
  cmake_modules
  dynamic_reconfigure
  dvo_core
  )

find_package(Eigen REQUIRED)

include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${Eigen_INCLUDE_DIRS}
  )

# dynamic reconfigure
generate_dynamic_reconfigure_options(cfg/dvo.cfg)

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES ${PROJECT_NAME} camera_tracker camera_tracker_nodelet
  CATKIN_DEPENDS roscpp nodelet
  )

add_definitions("-msse3")
add_definitions("-mfpmath=sse")

add_library(${PROJECT_NAME}
  src/camera_base.cpp
  src/camera_dense_tracking.cpp

  src/visualization/ros_camera_trajectory_visualizer.cpp
  )

target_link_libraries(${PROJECT_NAME}
  ${catkin_LIBRARIES}
  )

add_dependencies(${PROJECT_NAME}
  ${catkin_EXPORTED_TARGETS}
  )

add_library(camera_tracker_nodelet
  src/camera_tracker_nodelet.cpp
  )

target_link_libraries(camera_tracker_nodelet
  ${PROJECT_NAME}
  ${catkin_LIBRARIES}
  )

add_dependencies(camera_tracker_nodelet
  ${catkin_EXPORTED_TARGETS}
  )

add_executable(camera_tracker
  src/camera_tracker_node.cpp
  )

target_link_libraries(camera_tracker
  ${PROJECT_NAME}
  ${catkin_LIBRARIES}
  )

add_dependencies(camera_tracker
  ${catkin_EXPORTED_TARGETS}
  )