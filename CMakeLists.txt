# Top-level settings
cmake_minimum_required(VERSION 3.0.2)
project(graspit_interface)

# Enable automatic Qt MOC handling
set(CMAKE_AUTOMOC ON)
set(CMAKE_INCLUDE_CURRENT_DIR ON)

# Find catkin packages
find_package(catkin REQUIRED COMPONENTS
  message_generation
  std_msgs
  geometry_msgs
  sensor_msgs
  roscpp
  actionlib
  actionlib_msgs
)

# Find Qt5 components
find_package(Qt5 REQUIRED COMPONENTS Core Widgets)
set(QT_LIBRARIES Qt5::Core Qt5::Widgets)

# Find GraspIt (already cloned into workspace)
find_package(graspit)

# Declare message files
add_message_files(
  DIRECTORY msg
  FILES
  Body.msg
  GraspableBody.msg
  Robot.msg
  TactileSensorData.msg
  Grasp.msg
  Planner.msg
  SearchSpace.msg
  SearchContact.msg
  Energy.msg
  Contact.msg
  SimAnnParams.msg
)

# Declare service files
add_service_files(
  DIRECTORY srv
  FILES
  GetRobot.srv
  GetGraspableBody.srv
  GetBody.srv
  GetBodies.srv
  GetRobots.srv
  GetGraspableBodies.srv
  SetGraspableBodyPose.srv
  SetRobotPose.srv
  SetBodyPose.srv
  GetDynamics.srv
  SetDynamics.srv
  SetRobotDesiredDOF.srv
  ForceRobotDOF.srv
  MoveDOFToContacts.srv
  AutoGrasp.srv
  AutoOpen.srv
  ImportRobot.srv
  ImportObstacle.srv
  ImportGraspableBody.srv
  LoadWorld.srv
  SaveWorld.srv
  ClearWorld.srv
  SaveImage.srv
  ToggleAllCollisions.srv
  ComputeQuality.srv
  ComputeEnergy.srv
  ApproachToContact.srv
  FindInitialContact.srv
  DynamicAutoGraspComplete.srv
)

# Declare action files
add_action_files(
  DIRECTORY action
  FILES
  PlanGrasps.action
)

# Generate all ROS interfaces
generate_messages(
  DEPENDENCIES
  std_msgs
  geometry_msgs
  sensor_msgs
  actionlib_msgs
)

# Declare catkin package
catkin_package(
  INCLUDE_DIRS include
  CATKIN_DEPENDS geometry_msgs std_msgs message_runtime
)

# Include directories
include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${GRASPIT_INCLUDE_DIRS}
  ${Qt5Core_INCLUDE_DIRS}
  ${Qt5Widgets_INCLUDE_DIRS}
)

# Add main library with AUTOMOC enabled
add_library(graspit_interface
  src/graspit_interface.cpp
  src/main.cpp
)

# Add dependencies
add_dependencies(graspit_interface
  ${${PROJECT_NAME}_EXPORTED_TARGETS}
  ${catkin_EXPORTED_TARGETS}
)

# Link libraries
target_link_libraries(graspit_interface
  ${catkin_LIBRARIES}
  ${QT_LIBRARIES}
)

# Install headers (optional)
install(DIRECTORY include/graspit/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
  FILES_MATCHING PATTERN "*.h")


include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${Qt5Core_INCLUDE_DIRS}
  ${Qt5Widgets_INCLUDE_DIRS}
  ${CMAKE_SOURCE_DIR}/graspit/include
  # <-- Add this line
)
