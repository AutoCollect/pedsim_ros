cmake_minimum_required(VERSION 3.10)

# Set policies to avoid deprecation warnings
# cmake_policy(SET CMP0148 OLD)
# cmake_policy(SET CMP0167 OLD)

# Suppress developer warnings (same as using -Wno-dev on the command line)
set(CMAKE_SUPPRESS_DEVELOPER_WARNINGS 1 CACHE INTERNAL "Suppress developer warnings")

project(spencer_human_attribute_msgs)

# (Optional) Explicitly set the Python executable if needed.
set(PYTHON_EXECUTABLE "/usr/bin/python3")

find_package(catkin REQUIRED COMPONENTS
  roscpp
  roslib
  std_msgs
  message_generation
)

################################################
## Declare ROS messages, services and actions ##
################################################

# Generate messages in the 'msg' folder
add_message_files(
   FILES
   CategoricalAttribute.msg
   ScalarAttribute.msg
   HumanAttributes.msg
)

# Generate services in the 'srv' folder
#add_service_files(
#  FILES
#)

## Generate added messages and services with any dependencies listed here
generate_messages(
   DEPENDENCIES
   std_msgs
)

###################################
## catkin specific configuration ##
###################################
catkin_package(
  CATKIN_DEPENDS roscpp std_msgs message_runtime 
)
