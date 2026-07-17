# SPDX-FileCopyrightText: 2026, FANUC America Corporation
# SPDX-FileCopyrightText: 2026, FANUC CORPORATION
#
# SPDX-License-Identifier: Apache-2.0

cmake_minimum_required(VERSION 3.10)
project(fanuc_dual_arm_example)


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

install(DIRECTORY launch srdf urdf config
    DESTINATION share/${PROJECT_NAME}
)

install(PROGRAMS
    scripts/joint_state_merger.py
    DESTINATION lib/${PROJECT_NAME}
)

ament_python_install_package(${PROJECT_NAME})

ament_package()
