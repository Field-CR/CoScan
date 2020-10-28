cmake_minimum_required(VERSION 2.8.3)
project(co_scan)

find_package(catkin_simple REQUIRED)
catkin_simple()

add_definitions(-std=c++14 -Werror)
#set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Werror=unused-result -Werror=sign-compare -Wall  -march=native -Wreorder -Wno-pragma-once-outside-header -Werror -Wunused-value -Wno-unused-variable -Wno-unused-parameter -Wunknown-pragmas -Wunused-label -Wunknown-pragmas")
#set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Werror=unused-result -Werror=sign-compare -Wall -march=native -Wreorder -Wno-pragma-once-outside-header -Werror -Wunused-value -Wno-unused-variable -Wno-unused-parameter -Wunknown-pragmas -Wunused-label -Wunknown-pragmas")


## Compile as C++11, supported in ROS Kinetic and newer
# add_compile_options(-std=c++11)

#SET( EIGEN_INCLUDE_DIR /usr/include/eigen3 )

find_package(catkin REQUIRED COMPONENTS
  eigen_catkin
  roscpp
  rospy
  std_msgs
  camera_calibration_parsers
  cv_bridge
)

find_package(octomap REQUIRED)
find_package(CGAL REQUIRED)
find_package(OpenCV REQUIRED)

catkin_package(
#  INCLUDE_DIRS include
#  LIBRARIES co_scan
#  CATKIN_DEPENDS roscpp rospy std_msgs
#  DEPENDS system_lib
)

include_directories(
  ${catkin_INCLUDE_DIRS}
  ${OCTOMAP_INCLUDE_DIRS}
  ${CGAL_INCLUDE_DIRS}
)

add_library(data_engine src/data_engine.cpp)
add_library(navigation src/navigation.cpp)

add_executable(co_scan src/co_scan.cpp
src/global.cpp
src/geodesic/RichModel.cpp
src/geodesic/Xin_Wang.cpp
src/geodesic/DistanceApproach.cpp
src/geodesic/ExactDGPMethod.cpp
src/geodesic/BaseModel.cpp
src/geodesic/Chen_Han.cpp
src/geodesic/ICH_WindowFiltering.cpp
src/geodesic/Point3D.cpp
src/tsp/tsp.cpp
src/tsp/threads.cpp
src/tsp/MyThread.cpp
src/tsp/twoOpt.cpp
)

target_link_libraries(co_scan 
${catkin_LIBRARIES} 
${OCTOMAP_LIBRARIES}
#${CGAL_LIBRARIES}
CGAL::CGAL
CGAL::CGAL_Core
data_engine
navigation
#opencv_core, opencv_videoio, 
#opencv_highgui
${OpenCV_LIBS}
)

add_executable(poly_diff src/polygon_diff.cpp src/global.cpp)
target_link_libraries(poly_diff 
CGAL::CGAL CGAL::CGAL_Core
#opencv_core, opencv_videoio, 
#opencv_highgui
${OpenCV_LIBS}
)

