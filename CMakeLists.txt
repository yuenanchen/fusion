cmake_minimum_required(VERSION 3.0.2)
project(gnss_imu_fusion)


SET( CMAKE_CXX_FLAGS "-std=c++17 -O3" )

## Compile as C++11, supported in ROS Kinetic and newer
# add_compile_options(-std=c++11)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
  nav_msgs
  roscpp
  sensor_msgs
  std_msgs
  tf2_ros
  tf2

)
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "/usr/share/cmake/geographiclib/")
find_package (GeographicLib REQUIRED)
find_package(PCL REQUIRED)
catkin_package(
#  INCLUDE_DIRS include
#  LIBRARIES gnss_imu_fusion
#  CATKIN_DEPENDS nav_msgs roscpp sensor_msgs std_msgs
#  DEPENDS system_lib
CATKIN_DEPENDS roscpp nav_msgs geometry_msgs sensor_msgs tf2_ros tf2
)

include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${GeographicLib_INCLUDE_DIRS}
  ${PCL_INCLUDE_DIRS}
  lib
)
add_library(SC_ESKF 
    lib/SC_ESKF/src/eskf.cpp
    lib/SC_ESKF/src/SO3.cpp
    )
target_include_directories(SC_ESKF PRIVATE
    lib/SC_ESKF/include
    )
set(GNSS_IMU_FUSION_SRC_FILES
    src/fusion/gnss_imu_fusion.cpp
    src/wrapper/gnss_imu_fusion_wrapper.cpp
    src/fusion_node.cpp
)
add_executable(gnss_imu_fusion_node ${GNSS_IMU_FUSION_SRC_FILES})
target_link_libraries(gnss_imu_fusion_node
  SC_ESKF
  ${catkin_LIBRARIES}
  ${GeographicLib_LIBRARIES}
)

