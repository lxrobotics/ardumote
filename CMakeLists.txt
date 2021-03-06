cmake_minimum_required(VERSION 2.8.3)
project(ardumote)

find_package(catkin REQUIRED COMPONENTS
  message_generation
  rosserial_arduino
  rosserial_client
  std_msgs
  roscpp
)

################################################
## Declare ROS messages, services and actions ##
################################################

add_message_files(
  FILES
  PPM.msg
)

generate_messages(
)

################################################
## Declare ROS dynamic reconfigure parameters ##
################################################


###################################
## catkin specific configuration ##
###################################
catkin_package(
#  INCLUDE_DIRS include
#  LIBRARIES ardumote
  CATKIN_DEPENDS message_runtime rosserial_arduino rosserial_client std_msgs
#  DEPENDS system_lib
)

###########
## Build ##
###########

include_directories(
  ${catkin_INCLUDE_DIRS}
)

rosserial_generate_ros_lib(
  PACKAGE rosserial_arduino
  SCRIPT make_libraries.py
)

rosserial_configure_client(
  DIRECTORY firmware
  TOOLCHAIN_FILE ${ROSSERIAL_ARDUINO_TOOLCHAIN}
)

rosserial_add_client_target(firmware ardumote ALL)
rosserial_add_client_target(firmware ardumote-upload)

add_executable(ardumotetest 
  src/ardumotetest.cpp
)

target_link_libraries(ardumotetest
  ${catkin_LIBRARIES}
)

#############
## Install ##
#############


#############
## Testing ##
#############

