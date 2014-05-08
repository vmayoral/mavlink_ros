cmake_minimum_required(VERSION 2.8.3)
project(mavlink_ros)

list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)

find_package(catkin REQUIRED COMPONENTS
  rospy roscpp geometry_msgs sensor_msgs std_msgs message_generation
)

find_package(GLIB2  REQUIRED)
find_package(GTHREAD2  REQUIRED)

add_message_files(
  FILES
  Mavlink.msg
)

generate_messages(   
  DEPENDENCIES  
  std_msgs  
)  

catkin_package(
  INCLUDE_DIRS include
  CATKIN_DEPENDS rospy roscpp geometry_msgs sensor_msgs std_msgs message_runtime
)

set(MAVLINK_INCLUDE_DIR include/mavlink/v1.0/common)

include_directories(include)
include_directories(
  ${catkin_INCLUDE_DIRS}
  ${MAVLINK_INCLUDE_DIR}
  ${GLIB2_MAIN_INCLUDE_DIR}
  ${GLIB2_INTERNAL_INCLUDE_DIR}
  ${GTHREAD2_MAIN_INCLUDE_DIR}
)

## Declare a cpp executable
add_executable(mavlink_ros_serial src/mavlink_ros_serial.cpp)

## Specify libraries to link a library or executable target against
target_link_libraries(mavlink_ros_serial
  ${GLIB2_LIBRARY}
  ${GTHREAD2_LIBRARY}
  ${catkin_LIBRARIES}
)
