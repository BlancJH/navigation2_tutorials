cmake_minimum_required(VERSION 3.5)
project(nav2_gradient_costmap_plugin)

# ------------------------------
# Compiler settings
# ------------------------------
if(NOT CMAKE_C_STANDARD)
  set(CMAKE_C_STANDARD 99)
endif()
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()
if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# ------------------------------
# Find dependencies
# ------------------------------
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(rclcpp_lifecycle REQUIRED)
find_package(nav2_costmap_2d REQUIRED)
find_package(nav2_util REQUIRED)
find_package(pluginlib REQUIRED)

# ------------------------------
# Build the plugin library
# ------------------------------
add_library(${PROJECT_NAME}_core SHARED
  src/gradient_layer.cpp
)

target_include_directories(${PROJECT_NAME}_core PUBLIC
  include
)

ament_target_dependencies(${PROJECT_NAME}_core
  rclcpp
  rclcpp_lifecycle
  nav2_costmap_2d
  nav2_util
  pluginlib
)

# ------------------------------
# Export plugin description
# ------------------------------
# This will install gradient_layer.xml under share/nav2_costmap_2d
pluginlib_export_plugin_description_file(nav2_costmap_2d gradient_layer.xml)

# ------------------------------
# Install
# ------------------------------
install(
  TARGETS ${PROJECT_NAME}_core
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION lib
)

ament_package()
