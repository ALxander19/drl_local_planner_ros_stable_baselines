cmake_minimum_required(VERSION 2.8.3)
project(rl_msgs)

# Ensure we're using c++14
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++14")

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
  std_msgs
  geometry_msgs
  nav_msgs
  sensor_msgs
  message_generation
  roscpp
)

## System dependencies are found with CMake's conventions
find_package(PkgConfig REQUIRED)

find_package(PCL 1.3 REQUIRED COMPONENTS common io)

################
## Build MSGS ##
################
add_message_files(FILES
  Waypoint.msg
)

add_service_files(FILES
  MergeScans.srv
  SetPath.srv
  StateImageGenerationSrv.srv
)

generate_messages(
  DEPENDENCIES
  std_msgs
  geometry_msgs
  nav_msgs 
  sensor_msgs
  rl_msgs
)

###################################
## catkin specific configuration ##
###################################
catkin_package(
  INCLUDE_DIRS
  CATKIN_DEPENDS 
  std_msgs 
  geometry_msgs 
  nav_msgs 
  sensor_msgs 
  message_runtime
)

###########
## Build ##
###########

include_directories(
  ${catkin_INCLUDE_DIRS} ${PCL_INCLUDE_DIRS}
)

# for plc to work
link_directories(${PCL_LIBRARY_DIRS})
add_definitions(${PCL_DEFINITIONS})

add_executable(algo src/algo.cpp)
target_link_libraries(algo ${catkin_LIBRARIES})
