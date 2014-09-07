cmake_minimum_required(VERSION 2.8.3)
project(gmock_catkin)

find_package(catkin_simple REQUIRED)

catkin_simple()

include(ExternalProject)

file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include)

ExternalProject_Add(gmock_src
  URL https://googlemock.googlecode.com/files/gmock-1.7.0.zip
  UPDATE_COMMAND ""
  CONFIGURE_COMMAND cd ../gmock_src && cmake .
  BUILD_COMMAND cd ../gmock_src && make -j8
  INSTALL_COMMAND cp ../gmock_src/libgmock.a ${CATKIN_DEVEL_PREFIX}/lib/ &&
		  cp ../gmock_src/libgmock_main.a ${CATKIN_DEVEL_PREFIX}/lib/ &&
		  cp ../gmock_src/gtest/libgtest.a ${CATKIN_DEVEL_PREFIX}/lib/ &&
		  cp ../gmock_src/gtest/libgtest_main.a ${CATKIN_DEVEL_PREFIX}/lib/ &&
		  cp ../gmock_src/include/gmock ${CATKIN_DEVEL_PREFIX}/include/ -rf &&
		  cp ../gmock_src/gtest/include/gtest ${CATKIN_DEVEL_PREFIX}/include/ -rf
)

cs_add_library(${PROJECT_NAME} src/dependency_tracker.cc)
add_dependencies(${PROJECT_NAME} gmock_src)
target_link_libraries(${PROJECT_NAME} 
  ${CATKIN_DEVEL_PREFIX}/lib/libopencv_core${CMAKE_SHARED_LIBRARY_SUFFIX})

cs_install()

cs_export(
  INCLUDE_DIRS ${CATKIN_DEVEL_PREFIX}/include
  CFG_EXTRAS gmock-extras.cmake)
