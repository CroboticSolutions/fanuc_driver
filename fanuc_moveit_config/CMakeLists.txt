# SPDX-FileCopyrightText: 2025, FANUC America Corporation
# SPDX-FileCopyrightText: 2025, FANUC CORPORATION
#
# SPDX-License-Identifier: Apache-2.0

cmake_minimum_required(VERSION 3.10)
project(fanuc_moveit_config)


# Do NOT set CMP0167 NEW here: it breaks transitive geometric_shapes →
# random_numbers::random_numbers (classic FindBoost path used by MoveIt extras).

set(CMAKE_CXX_STANDARD 17)

find_package(ament_cmake REQUIRED)
find_package(random_numbers REQUIRED)
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
