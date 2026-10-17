cmake_minimum_required(VERSION 2.8.3)
project(yikun_msgs)

## Compile as C++11, supported in ROS Kinetic and newer
# add_compile_options(-std=c++11)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
  actionlib
  actionlib_msgs
  geometry_msgs
  message_generation
  nav_msgs
  roscpp
  rospy
  std_msgs
)

## System dependencies are found with CMake's conventions
# find_package(Boost REQUIRED COMPONENTS system)

## Generate messages in the 'msg' folder
add_message_files(
  FILES
  Num.msg
  Application.msg
  AppStatus.msg
  AppUpdate.msg
  Image.msg
  LineSegment.msg
  LineSegmentList.msg
  DockPose.msg
  PosCss.msg
  #sbpl
  SBPLLatticePlannerStats.msg
  #costmap
  VoxelGrid.msg
  #teb
  FeedbackMsg.msg
  ObstacleMsg.msg
  TrajectoryMsg.msg
  TrajectoryPointMsg.msg
  Wifi.msg
  WifiList.msg
  Dock.msg
  BestWayFeedback.msg
)

## Generate services in the 'srv' folder
add_service_files(
  FILES
  #apps
  Ismapping.srv
  Localization.srv
  SaveMap.srv
  ListTasks.srv
  StartTask.srv
  StatusUpdate.srv
  StopTask.srv
  Location.srv
  SetPose.srv
  SetState.srv
  GetState.srv
  URCommand.srv
  ModbusCmd.srv
  SetWifi.srv
  ComWithWeb.srv
  Isreading.srv   #by syz
  TaskPojo.srv
  ControlPojo.srv
  SafetyLaser.srv #by syz 安全激光控制
  BestWayForWeb.srv
  SearchOptimalWayTask.srv #by syz 搜索最短路径
  ConfidenceLevel.srv  #by syz 置信度控制
  ControllerSwitch.srv  #by syz 统一的(功能)控制开关,现在包含了安全激光\置信度\防跌落
  IndustralPath.srv
)

## Generate actions in the 'action' folder
add_action_files(
  FILES
  #apps
  RelativeMove.action
  MapSave.action
  AutoDock.action
  Dock.action
  #movebase
  MoveBase.action
  CameraControl.action
  TakePictures.action
  PutterPosition.action
  AiHttpPost.action
  #industral task
  IndustralPathTask.action
  IndustralPathCss.action
  #号点对齐
  PointAlignment.action
)

## Generate added messages and services with any dependencies listed here
generate_messages(
  DEPENDENCIES
  actionlib_msgs
  geometry_msgs
  nav_msgs
  std_msgs
)

catkin_package(
  CATKIN_DEPENDS
    actionlib_msgs
    geometry_msgs
    message_runtime
    nav_msgs
    std_msgs
)

include_directories(
# include
  ${catkin_INCLUDE_DIRS}
)
