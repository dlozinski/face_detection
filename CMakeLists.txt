cmake_minimum_required(VERSION 2.8.3)
project(face_detection)

find_package(catkin REQUIRED COMPONENTS
  cv_bridge
  image_transport
  roscpp
  rospy
  sensor_msgs
  std_msgs
  dynamic_reconfigure
)

find_package(OpenCV REQUIRED)

generate_dynamic_reconfigure_options(
  cfg/face_det.cfg
  cfg/face_det_dlib.cfg
  cfg/face_det_cuda.cfg
  cfg/face_track.cfg
  #cfg/face_track_cuda.cfg
)

catkin_package(
#  INCLUDE_DIRS include
#  LIBRARIES face_detection
   CATKIN_DEPENDS cv_bridge image_transport roscpp rospy sensor_msgs std_msgs dynamic_reconfigure
#  DEPENDS system_lib
)

include_directories(
  ${catkin_INCLUDE_DIRS}
  ${OpenCV_INCLUDE_DIRS}
)


## Declare a cpp library
# add_library(face_detection
#   src/${PROJECT_NAME}/face_detection.cpp
# )

##############################
####### DLIB SECTION #########
##############################

# set this value to the directory of you DLib installation
# for example: include(/home/user/dlib-X.X/dlib/cmake)
#include(../dlib/cmake)

#set(USE_AVX_INSTRUCTIONS 1)


#add_executable(face_detection_dlib src/face_detection_dlib.cpp)
#add_dependencies(face_detection_dlib ${PROJECT_NAME}_gencfg)
#target_link_libraries(face_detection_dlib dlib ${catkin_LIBRARIES})


## Declare a cpp executable

add_executable(face_detection src/face_detection.cpp)
add_executable(face_tracking src/face_tracking.cpp)
add_executable(face_listener src/face_listener.cpp)
#add_executable(face_detection_cuda src/face_detection_cuda.cpp)
#add_executable(face_tracking_cuda src/face_tracking_cuda.cpp)

add_dependencies(face_detection ${PROJECT_NAME}_gencfg)
add_dependencies(face_tracking ${PROJECT_NAME}_gencfg)

#add_dependencies(face_detection_cuda ${PROJECT_NAME}_gencfg)
#add_dependencies(face_tracking_cuda ${PROJECT_NAME}_gencfg)

target_link_libraries(face_detection
   ${catkin_LIBRARIES}
   ${OpenCV_LIBS}
)

target_link_libraries(face_tracking
   ${catkin_LIBRARIES}
   ${OpenCV_LIBS}
)

target_link_libraries(face_listener
   ${catkin_LIBRARIES}
)

#target_link_libraries(face_detection_cuda
#   ${catkin_LIBRARIES}
#)
#target_link_libraries(face_tracking_cuda
#   ${catkin_LIBRARIES}
#)


#############
## Install ##
#############

# all install targets should use catkin DESTINATION variables
# See http://ros.org/doc/api/catkin/html/adv_user_guide/variables.html

## Mark executable scripts (Python etc.) for installation
## in contrast to setup.py, you can choose the destination
# install(PROGRAMS
#   scripts/my_python_script
#   DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )

## Mark executables and/or libraries for installation
# install(TARGETS face_detection face_detection_node
#   ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )

## Mark cpp header files for installation
# install(DIRECTORY include/${PROJECT_NAME}/
#   DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
#   FILES_MATCHING PATTERN "*.h"
#   PATTERN ".svn" EXCLUDE
# )

## Mark other files for installation (e.g. launch and bag files, etc.)
# install(FILES
#   # myfile1
#   # myfile2
#   DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
# )

#############
## Testing ##
#############

## Add gtest based cpp test target and link libraries
# catkin_add_gtest(${PROJECT_NAME}-test test/test_face_detection.cpp)
# if(TARGET ${PROJECT_NAME}-test)
#   target_link_libraries(${PROJECT_NAME}-test ${PROJECT_NAME})
# endif()

## Add folders to be run by python nosetests
# catkin_add_nosetests(test)
