cmake_minimum_required(VERSION 2.8.3)
project(jsk_recognition_utils)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
  jsk_recognition_msgs
  pcl_ros
  pcl_msgs
  eigen_conversions
  tf_conversions
  tf
  tf2_ros
  image_geometry
  sensor_msgs
  geometry_msgs
  jsk_topic_tools
)

catkin_package(
 INCLUDE_DIRS include
 LIBRARIES jsk_recognition_utils
 CATKIN_DEPENDS jsk_recognition_msgs pcl_ros
)

find_package(OpenCV REQUIRED core imgproc)
find_package(PkgConfig)
pkg_check_modules(yaml_cpp yaml-cpp REQUIRED)
IF(${yaml_cpp_VERSION} VERSION_LESS "0.5.0")
## indigo yaml-cpp : 0.5.0 /  hydro yaml-cpp : 0.3.0
  add_definitions("-DUSE_OLD_YAML")
ENDIF()

include_directories(
  ${catkin_INCLUDE_DIRS} include
)
link_libraries(${catkin_LIBRARIES} ${pcl_ros_LIBRARIES} ${OpenCV_LIBRARIES}  yaml-cpp)

add_library(jsk_recognition_utils SHARED
  src/grid_index.cpp
  src/grid_map.cpp 
  src/grid_line.cpp 
  src/geo_util.cpp
  src/random_util.cpp
  src/pcl_ros_util.cpp
  src/pcl_conversion_util.cpp 
  src/pcl_util.cpp
  src/tf_listener_singleton.cpp
  src/pcl/ear_clipping_patched.cpp
  )

install(TARGETS jsk_recognition_utils
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION})