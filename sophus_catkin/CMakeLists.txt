cmake_minimum_required(VERSION 2.8.3)
project(sophus)

IF( NOT CMAKE_BUILD_TYPE )
   SET( CMAKE_BUILD_TYPE Release )
ENDIF()

find_package(catkin REQUIRED COMPONENTS
  cmake_modules)

find_package( Eigen REQUIRED )

include_directories(
  ${PROJECT_SOURCE_DIR}
  ${catkin_INCLUDE_DIRS}
  ${Eigen_INCLUDE_DIRS}
  )

add_definitions(${Eigen_DEFINITIONS})

catkin_package(
  INCLUDE_DIRS ${PROJECT_SOURCE_DIR}
  LIBRARIES ${PROJECT_NAME}
  )

ADD_LIBRARY(${PROJECT_NAME}
  sophus/so2.cpp
  sophus/se2.cpp
  sophus/se3.cpp
  sophus/so3.cpp
  sophus/scso3.cpp
  sophus/sim3
  )

install(TARGETS ${PROJECT_NAME}
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  )

install(DIRECTORY sophus/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
  FILES_MATCHING PATTERN "*.h*"
  PATTERN ".git" EXCLUDE
  )
