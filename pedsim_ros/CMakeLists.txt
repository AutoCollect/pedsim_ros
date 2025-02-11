cmake_minimum_required(VERSION 2.8.3)
project(pedsim_ros)

# Append our local cmake modules folder so that our stub FindPythonInterp is found.
list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake/Modules")

# (Optional) Explicitly set the Python executable if needed.
set(PYTHON_EXECUTABLE "/usr/bin/python3")

find_package(catkin REQUIRED)
catkin_metapackage()
