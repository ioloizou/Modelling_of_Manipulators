cmake_minimum_required(VERSION 3.5)
project(ecn_manip)

set(CMAKE_BUILD_TYPE Debug)

find_package(catkin REQUIRED COMPONENTS
sensor_msgs
tf2_ros
urdf
)
  
find_package(VISP REQUIRED COMPONENTS core)

set(CMAKE_CXX_STANDARD 17)
catkin_package(
  INCLUDE_DIRS src
  LIBRARIES ecn_manip
  CATKIN_DEPENDS roscpp sensor_msgs tf2_ros urdf
  DEPENDS VISP)

include_directories(include src ${VISP_INCLUDE_DIRS} ${catkin_INCLUDE_DIRS})

# main library
add_library(ecn_manip src/robot_base.cpp src/trig_solvers.cpp src/ur_kin/ur_kin.cpp src/node.cpp)
target_link_libraries(ecn_manip ${VISP_LIBRARIES} ${catkin_LIBRARIES})

# Main executable
add_executable(control control.cpp
              robot_turret.cpp
               robot_ur10.cpp
               robot_kr16.cpp
               )
target_link_libraries(control ecn_manip)


# Other executables
#add_executable(test_solvers src/test_solvers.cpp)
#target_link_libraries(test_solvers ecn_manip)

add_executable(example_RRRP example/robot_rrrp.h example/robot_rrrp.cpp example/rrrp_solver.cpp)
target_link_libraries(example_RRRP ecn_manip)
