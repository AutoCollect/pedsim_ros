cmake_minimum_required(VERSION 2.8.3)

# Set policies to avoid deprecation warnings
cmake_policy(SET CMP0148 NEW)
cmake_policy(SET CMP0167 NEW)

# Suppress developer warnings (same as using -Wno-dev on the command line)
set(CMAKE_SUPPRESS_DEVELOPER_WARNINGS 1 CACHE INTERNAL "Suppress developer warnings")

project(pedsim)

# Append our local cmake modules folder so that our stub FindPythonInterp is found.
list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake/Modules")

# (Optional) Explicitly set the Python executable if needed.
set(PYTHON_EXECUTABLE "/usr/bin/python3")

#------------------ Configuration ------------------#
option(SHALL_DEBUG "Enable debug features" OFF)
option(SHALL_PROFILE "Enable the code profiling feature" OFF)
option(CMAKE_VERBOSE_MAKEFILE "Full compiler output" ON)


#----------------- Compiler Flags ------------------#
add_definitions(-Wall -Wunused -std=c++0x -pipe)

#debugging
if(SHALL_DEBUG)
  message("Debugging activated")
  add_definitions(-O0 -DDEBUG -ggdb -g3 -rdynamic)
else(SHALL_DEBUG)
  message("Debugging deactivated")
  add_definitions(-Os)
endif(SHALL_DEBUG)


find_package(catkin REQUIRED COMPONENTS
  roscpp
)
find_package(Boost REQUIRED)

catkin_package(
  CATKIN_DEPENDS  roscpp
  INCLUDE_DIRS ${PROJECT_SOURCE_DIR}/include/
  LIBRARIES pedsim
)

include_directories(
  ${catkin_INCLUDE_DIRS}
  ${PROJECT_SOURCE_DIR}/include/pedsim/
  ${roscpp_INCLUDE_DIRS}
)

set(SOURCES
  src/ped_agent.cpp
  src/ped_angle.cpp
  src/ped_obstacle.cpp
  src/ped_scene.cpp
  src/ped_tree.cpp
  src/ped_vector.cpp
  src/ped_waypoint.cpp
)

add_library(pedsim ${SOURCES})

target_link_libraries(pedsim
  ${BOOST_LIBRARIES}
)
