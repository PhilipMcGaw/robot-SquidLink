cmake_minimum_required(VERSION 3.8)
project(testbot_description)

find_package(ament_cmake REQUIRED)
find_package(ament_index_python REQUIRED)
find_package(robot_state_publisher REQUIRED)
find_package(xacro REQUIRED)

install(
  DIRECTORY launch urdf
  DESTINATION share/${PROJECT_NAME}
)

ament_package()