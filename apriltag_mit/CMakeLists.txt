cmake_minimum_required(VERSION 3.5)
project(apriltag_mit)

set(CMAKE_CXX_STANDARD 11)

find_package(Eigen3 REQUIRED)
find_package(OpenCV REQUIRED COMPONENTS core calib3d)
find_package(Boost REQUIRED)
find_package(ament_cmake REQUIRED)
find_package(ament_cmake_auto REQUIRED)

ament_auto_find_build_dependencies()

include_directories(include ${OpenCV_INCLUDE_DIRS} ${EIGEN3_INCLUDE_DIR} ${Boost_INCLUDE_DIRS})

file(GLOB CC_FILES ${PROJECT_SOURCE_DIR}/src/*.cc)
add_library(${PROJECT_NAME} SHARED ${CC_FILES})
target_include_directories(${PROJECT_NAME} PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>)
target_link_libraries(${PROJECT_NAME} PUBLIC Eigen3::Eigen opencv_core opencv_calib3d ${Boost_LIBRARIES})

install(TARGETS ${PROJECT_NAME}
  EXPORT export_${PROJECT_NAME}
  DESTINATION lib
  INCLUDES DESTINATION include)

install(DIRECTORY include/${PROJECT_NAME}/
  DESTINATION include/${PROJECT_NAME})

ament_export_targets(export_${PROJECT_NAME})
ament_export_dependencies(Eigen3 OpenCV Boost)
ament_package()
