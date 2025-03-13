cmake_minimum_required(VERSION 3.10)

# Set policies to avoid deprecation warnings
# cmake_policy(SET CMP0148 OLD)
# cmake_policy(SET CMP0167 OLD)

# Suppress developer warnings (same as using -Wno-dev on the command line)
set(CMAKE_SUPPRESS_DEVELOPER_WARNINGS 1 CACHE INTERNAL "Suppress developer warnings")

project(pedsim_srvs)

# (Optional) Explicitly set the Python executable if needed.
set(PYTHON_EXECUTABLE "/usr/bin/python3")

set( MESSAGE_DEPENDENCIES
    pedsim_msgs
    std_msgs
)

find_package(catkin REQUIRED 
  COMPONENTS
  message_generation
  ${MESSAGE_DEPENDENCIES}
)

include_directories(${catkin_INCLUDE_DIRS})
link_directories(${catkin_LIBRARY_DIRS})


# the services
add_service_files( DIRECTORY srv
  FILES
  SetAgentState.srv
  GetAgentState.srv
  SetAllAgentsState.srv
  GetAllAgentsState.srv
)

generate_messages(DEPENDENCIES ${MESSAGE_DEPENDENCIES})

#add_dependencies(some_target ${PROJECT_NAME}_gencpp)

catkin_package( CATKIN_DEPENDS message_runtime ${MESSAGE_DEPENDENCIES})