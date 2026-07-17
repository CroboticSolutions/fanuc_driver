# SPDX-FileCopyrightText: 2025, FANUC America Corporation
# SPDX-FileCopyrightText: 2025, FANUC CORPORATION
#
# SPDX-License-Identifier: Apache-2.0

cmake_minimum_required(VERSION 3.10)
project(fanuc_moveit_config)


# Silence CMP0167 dev warnings from MoveIt config extras that still call
# find_package(Boost): use Boost's own CMake config instead of the removed
# FindBoost module.
if(POLICY CMP0167)
  cmake_policy(SET CMP0167 NEW)
endif()

set(CMAKE_CXX_STANDARD 17)

find_package(ament_cmake REQUIRED)
find_package(moveit_ros_control_interface REQUIRED)
find_package(pluginlib REQUIRED)

add_library(fanuc_controllers_moveit_trajectory_plugin SHARED
    src/fanuc_controllers_moveit_trajectory_plugin.cpp
)
target_link_libraries(fanuc_controllers_moveit_trajectory_plugin PRIVATE
    moveit_ros_control_interface::moveit_ros_control_interface_trajectory_plugin)

pluginlib_export_plugin_description_file(moveit_ros_control_interface fanuc_controllers_moveit_trajectory_plugin.xml)

install(FILES
    fanuc_controllers_moveit_trajectory_plugin.xml
    DESTINATION share/${PROJECT_NAME}
)
install(TARGETS
    fanuc_controllers_moveit_trajectory_plugin
    DESTINATION lib/${PROJECT_NAME}
)

install(DIRECTORY config launch rviz srdf
    DESTINATION share/${PROJECT_NAME}
)

ament_package()
