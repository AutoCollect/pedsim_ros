cmake_minimum_required(VERSION 3.10)

# Suppress developer warnings (equivalent to using -Wno-dev on the command line)
set(CMAKE_SUPPRESS_DEVELOPER_WARNINGS 1 CACHE INTERNAL "Suppress developer warnings")

# Set policies to avoid deprecation warnings
cmake_policy(SET CMP0148 NEW)
cmake_policy(SET CMP0167 NEW)

project(pedsim_gazebo_plugin VERSION 0.1.0 LANGUAGES CXX)

# Append our local cmake modules folder so that our stub FindPythonInterp is found.
list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake/Modules")

# (Optional) Explicitly set the Python executable if needed.
set(PYTHON_EXECUTABLE "/usr/bin/python3")

# Force C++17
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

##########################
## Force System Protobuf
##########################
# These settings force the use of the system Protobuf (libprotoc 3.6.1) over any other.
set(PROTOBUF_PROTOC_EXECUTABLE "/usr/bin/protoc" CACHE STRING "Path to protoc" FORCE)
set(Protobuf_INCLUDE_DIR "/usr/include" CACHE STRING "Protobuf include directory" FORCE)
set(Protobuf_LIBRARIES "/usr/lib/x86_64-linux-gnu/libprotobuf.so" CACHE STRING "Protobuf library" FORCE)

find_package(Protobuf REQUIRED)

# Create an imported target for the system Protobuf.
add_library(protobuf::libprotobuf UNKNOWN IMPORTED)
set_target_properties(protobuf::libprotobuf PROPERTIES
  IMPORTED_LOCATION "${Protobuf_LIBRARIES}"
  INTERFACE_INCLUDE_DIRECTORIES "${Protobuf_INCLUDE_DIR}"
)

##########################
## Find catkin and ROS packages
##########################
find_package(catkin REQUIRED COMPONENTS
  gazebo_ros
  geometry_msgs
  roscpp
  rospy
  gazebo_msgs
  pedsim_msgs
)

find_package(gazebo REQUIRED)

##########################
## Declare catkin package
##########################
catkin_package(
  CATKIN_DEPENDS gazebo_ros roscpp geometry_msgs pedsim_msgs
)

##########################
## Include directories
##########################
include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${GAZEBO_INCLUDE_DIRS}
  ${Protobuf_INCLUDE_DIR}  # for protobuf includes
)

##########################
## Build the Plugin Library
##########################
add_library(ActorPosesPlugin src/actor_poses_plugin.cpp)

target_link_libraries(ActorPosesPlugin 
  ${catkin_LIBRARIES}
  ${GAZEBO_LIBRARIES}
  protobuf::libprotobuf
)

add_dependencies(ActorPosesPlugin ${catkin_EXPORTED_TARGETS})

##########################
## Install targets (optional)
##########################
install(TARGETS ActorPosesPlugin
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
)
install(DIRECTORY include/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
)