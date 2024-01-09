cmake_minimum_required(VERSION 3.0.2)
project(z1_controller LANGUAGES CXX)
set(PACKAGE_NAME z1_controller)
set(CMAKE_CXX_FLAGS "$ENV{CXXFLAGS} -O3 -std=c++14 -pthread")


## Compile as C++11, supported in ROS Kinetic and newer
# add_compile_options(-std=c++11)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages

find_package(Boost REQUIRED)
find_package(Eigen3 REQUIRED)
find_package(catkin REQUIRED COMPONENTS
  controller_manager
  # genmsg
  joint_state_controller
  robot_state_publisher
  roscpp
  gazebo_ros
  std_msgs
  tf
  geometry_msgs
  urdf
  xacro
)

catkin_package(
 CATKIN_DEPENDS
 controller_manager
 joint_state_controller
 robot_state_publisher
 roscpp
 gazebo_ros
 std_msgs
 tf
 geometry_msgs
 urdf
 xacro
)

include_directories(
  ${catkin_INCLUDE_DIRS}
  ${GAZEBO_INCLUDE_DIRS}
  include
)

link_directories(lib
  ${GAZEBO_LIBRARY_DIRS}
)

add_executable(sim_ctrl src/sim_ctrl.cpp src/IOROS.cpp)
target_link_libraries(sim_ctrl ${catkin_LIBRARIES} libZ1_${CMAKE_HOST_SYSTEM_PROCESSOR}.so)
# target_link_libraries(sim_ctrl ${catkin_LIBRARIES} libUnitreeArm_${CMAKE_HOST_SYSTEM_PROCESSOR}.so)
# ---------------------------add executable-----------
# set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR})
# add_executable(sim_ctrl src/sim_ctrl.cpp)
# target_link_libraries(sim_ctrl ${catkin_LIBRARIES})

# target_link_directories(sim_ctrl 
#   libZ1_${CMAKE_HOST_SYSTEM_PROCESSOR}.so
#   ${catkin_LIBRARIES}
# )


# install 경로 지정
# 24.01.08 install 경로 지정 -> directory
# https://enssionaut.com/board_robotics/1264
install(TARGETS sim_ctrl
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(DIRECTORY launch/
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/launch
)

################ directory 확
install(DIRECTORY include
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
  PATTERN ".svn" EXCLUDE
)


# include_directories(
#   ${catkin_INCLUDE_DIRS}
#   include
# )