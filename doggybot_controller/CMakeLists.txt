cmake_minimum_required(VERSION 3.8)
project(doggybot_controller)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(pcl_conversions REQUIRED)

include_directories( include )
add_library( controller SHARED src/controller_node.cpp 
                               src/PP.cpp
                               src/PID.cpp)
ament_target_dependencies( controller rclcpp geometry_msgs sensor_msgs pcl_conversions)

add_executable( control src/controller_node.cpp 
                        src/PP.cpp
                        src/PID.cpp)
target_link_libraries( control controller )
ament_target_dependencies( control rclcpp )

install( DIRECTORY include/ DESTINATION include)
install( TARGETS controller LIBRARY DESTINATION lib )
install( TARGETS control DESTINATION lib/${PROJECT_NAME} )


if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # the following line skips the linter which checks for copyrights
  # comment the line when a copyright and license is added to all source files
  set(ament_cmake_copyright_FOUND TRUE)
  # the following line skips cpplint (only works in a git repo)
  # comment the line when this package is in a git repo and when
  # a copyright and license is added to all source files
  set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
