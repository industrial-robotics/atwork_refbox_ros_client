cmake_minimum_required(VERSION 2.8.7)
project(atwork_refbox_ros_client)


find_package(
  Boost REQUIRED COMPONENTS system filesystem thread)
find_package(catkin REQUIRED
  COMPONENTS
    roscpp
    protobuf_comm
    atwork_pb_msgs
    atwork_ros_msgs
    std_msgs
    geometry_msgs
    message_generation
)
find_package(Protobuf)

SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -std=gnu++0x -DHAVE_LIBCRYPTO")

include_directories(
  ros/include
  ${catkin_INCLUDE_DIRS}
)

catkin_package(
  CATKIN_DEPENDS
    roscpp
    protobuf_comm
    atwork_pb_msgs
    atwork_ros_msgs
)

add_executable(robot_example_ros
   ros/src/robot_example_ros_node.cpp
   ros/src/robot_example_ros.cpp
)

target_link_libraries(robot_example_ros
   ${catkin_LIBRARIES}
   ${Boost_LIBRARIES}
   ${PROTOBUF_LIBRARIES}
)

add_dependencies(robot_example_ros atwork_ros_msgs_generate_messages_cpp)


install(
  DIRECTORY ros/include/${PROJECT_NAME}/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
  FILES_MATCHING PATTERN "*.h"
)

install(
  TARGETS robot_example_ros
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)


