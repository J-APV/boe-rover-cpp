cmake_minimum_required(VERSION 2.8.3)
project(ur50_lidar_driver)

add_compile_options(-std=c++14)
set( CMAKE_CXX_STANDARD 14)


find_package(catkin REQUIRED COMPONENTS
  roscpp
  diagnostic_updater
  nodelet
  ur50_lidar_msgs
)

find_package(Boost REQUIRED)

catkin_package(
  INCLUDE_DIRS include
#  LIBRARIES ur50_lidar_driver
  CATKIN_DEPENDS
    roscpp diagnostic_updater nodelet
    ur50_lidar_msgs
  DEPENDS
    boost
)

include_directories(
  include
  ${Boost_INCLUDE_DIR}
  ${catkin_INCLUDE_DIRS}
)

link_directories(
  ${catkin_LIBRARY_DIRS}
)

# Leishen n301 lidar driver
add_library(ur50_lidar_driver
  src/ur50_lidar_driver.cc
)
target_link_libraries(ur50_lidar_driver
  ${catkin_LIBRARIES}
)
add_dependencies(ur50_lidar_driver
  ${${PROJECT_NAME}_EXPORTED_TARGETS}
  ${catkin_EXPORTED_TARGETS}
)

# Leishen n301 lidar nodelet
add_executable(ur50_lidar_driver_node
  src/ur50_lidar_driver_node.cc
)
target_link_libraries(ur50_lidar_driver_node
  ur50_lidar_driver
  ${catkin_LIBRARIES}
)
add_dependencies(ur50_lidar_driver_node
  ${${PROJECT_NAME}_EXPORTED_TARGETS}
  ${catkin_EXPORTED_TARGETS}
)

# install options
install(TARGETS ur50_lidar_driver_node
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(DIRECTORY launch
        DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)

