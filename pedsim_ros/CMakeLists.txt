cmake_minimum_required(VERSION 3.10)

# Set policies to avoid deprecation warnings
cmake_policy(SET CMP0148 NEW)
cmake_policy(SET CMP0167 NEW)

# Suppress developer warnings (same as using -Wno-dev on the command line)
set(CMAKE_SUPPRESS_DEVELOPER_WARNINGS 1 CACHE INTERNAL "Suppress developer warnings")

project(pedsim_ros)

# Append our local cmake modules folder so that our stub FindPythonInterp is found.
list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake/Modules")

# (Optional) Explicitly set the Python executable if needed.
set(PYTHON_EXECUTABLE "/usr/bin/python3")

find_package(catkin REQUIRED)
catkin_metapackage()
