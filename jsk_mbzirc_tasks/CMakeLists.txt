cmake_minimum_required(VERSION 2.8.3)
project(jsk_mbzirc_tasks)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS roscpp gazebo_ros)

# Depend on system install of Gazebo and SDFormat
find_package(gazebo REQUIRED)

###################################
## catkin specific configuration ##
###################################
catkin_package(
    CATKIN_DEPENDS
)

include_directories(include ${catkin_INCLUDE_DIRS} ${GAZEBO_INCLUDE_DIRS})

add_executable(uav_teleop_keyboard src/uav_teleop_keyboard.cpp)
target_link_libraries(uav_teleop_keyboard ${catkin_LIBRARIES})

#############
## Install ##
#############
install(DIRECTORY launch urdf config
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
  USE_SOURCE_PERMISSIONS)


#############
## Testing ##
#############
if(CATKIN_ENABLE_TESTING)
  find_package(rostest REQUIRED)
  find_package(roslaunch REQUIRED)
  find_package(roslint REQUIRED)
  # https://github.com/ros/ros_comm/pull/730
  roslint_cpp()
  roslint_python()
  roslint_add_test()
  set(roslaunch_check_script ${PROJECT_SOURCE_DIR}/script/roslaunch-check.py)
  roslaunch_add_file_check(launch/jsk_mbzirc_task_1.launch)
  roslaunch_add_file_check(launch/jsk_mbzirc_task_2.launch)
  set(roslaunch_check_script ${roslaunch_DIR}/../scripts/roslaunch-check)
  #
  add_rostest(test/jsk_mbzirc_task_1.test)
  add_rostest(test/jsk_mbzirc_task_2.test)
endif()
