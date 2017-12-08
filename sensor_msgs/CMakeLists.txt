cmake_minimum_required(VERSION 2.8.3)
project(sensor_msgs)

find_package(catkin REQUIRED COMPONENTS message_generation std_msgs)

# For point_cloud2.py
catkin_python_setup()

include_directories(include)

add_message_files(
  DIRECTORY msg
  FILES
  BatteryState.msg
  CameraInfo.msg
  ChannelFloat32.msg
  CompressedImage.msg
  FluidPressure.msg
  Illuminance.msg
  Image.msg
  Imu.msg
  JointState.msg
  Joy.msg
  JoyFeedback.msg
  JoyFeedbackArray.msg
  LaserEcho.msg
  LaserScan.msg
  MagneticField.msg
  MultiDOFJointState.msg
  MultiEchoLaserScan.msg
  NavSatFix.msg
  NavSatStatus.msg
  PointCloud.msg
  PointCloud2.msg
  PointField.msg
  Range.msg
  RegionOfInterest.msg
  RelativeHumidity.msg
  Temperature.msg
  TimeReference.msg)

add_service_files(
  DIRECTORY srv
  FILES SetCameraInfo.srv)

generate_messages(DEPENDENCIES geometry_msgs std_msgs)

set(PROJECT_INSTALLSPACE_LIBRARIES ros::${PROJECT_NAME})
catkin_package(
  INCLUDE_DIRS include
  CATKIN_DEPENDS geometry_msgs message_runtime std_msgs)

add_library(${PROJECT_NAME} INTERFACE)
target_include_directories(${PROJECT_NAME} INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>)

install(DIRECTORY include/${PROJECT_NAME}/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
  FILES_MATCHING PATTERN "*.h")

install(TARGETS ${PROJECT_NAME}
  EXPORT ${PROJECT_NAME}Targets
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_GLOBAL_BIN_DESTINATION})

install(EXPORT ${PROJECT_NAME}Targets
    DESTINATION share/${PROJECT_NAME}/cmake
    NAMESPACE ros::
    FILE ${PROJECT_NAME}Targets.cmake
)

install(DIRECTORY migration_rules
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})

if (CATKIN_ENABLE_TESTING)
  add_subdirectory(test)
endif()
