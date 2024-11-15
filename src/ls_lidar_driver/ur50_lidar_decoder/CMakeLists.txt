cmake_minimum_required(VERSION 2.8.3)
project(ur50_lidar_decoder)

add_compile_options(-std=c++14)
set( CMAKE_CXX_STANDARD 14)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  pluginlib
  sensor_msgs
  pcl_ros
  pcl_conversions
  ur50_lidar_msgs
)
find_package(Boost REQUIRED)


catkin_package(
  INCLUDE_DIRS include
#  LIBRARIES ur50_lidar_decoder
  CATKIN_DEPENDS
    roscpp sensor_msgs pluginlib
    pcl_ros pcl_conversions
    ur50_lidar_msgs
  DEPENDS
    Boost
)

include_directories(
  include
  ${Boost_INCLUDE_DIR}
  ${catkin_INCLUDE_DIRS}
)

link_directories(
  ${catkin_LIBRARY_DIRS}
)

# Lslidar N301 Decoder
add_library(ur50_lidar_decoder
  src/ur50_lidar_decoder.cpp
)
target_link_libraries(ur50_lidar_decoder
  ${catkin_LIBRARIES}
)
add_dependencies(ur50_lidar_decoder
  ${${PROJECT_NAME}_EXPORTED_TARGETS}
  ${catkin_EXPORTED_TARGETS}
)

# Lslidar N301 Decoder node
add_executable(ur50_lidar_decoder_node
  src/ur50_lidar_decoder_node.cpp
)
target_link_libraries(ur50_lidar_decoder_node
  ur50_lidar_decoder
  ${catkin_LIBRARIES}
)
add_dependencies(ur50_lidar_decoder_node
  ${${PROJECT_NAME}_EXPORTED_TARGETS}
  ${catkin_EXPORTED_TARGETS}
)

install(TARGETS ur50_lidar_decoder_node
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(DIRECTORY launch
        DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)
