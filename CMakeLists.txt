cmake_minimum_required(VERSION 2.8.3)
project(gmock_catkin)

find_package(catkin_simple REQUIRED)

catkin_simple()

include(ExternalProject)

file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include)

ExternalProject_Add(gmock_src
  URL https://github.com/google/googlemock/archive/release-1.7.0.zip
  UPDATE_COMMAND ""
  CONFIGURE_COMMAND cd ../gmock_src && 
  	cmake . 
  	-DCMAKE_CXX_FLAGS=-fPIC 
  	-DBUILD_SHARED_LIBS=ON
  BUILD_COMMAND cd ../gmock_src && make -j8
  INSTALL_COMMAND
  	cp -a
		../gmock_src/libgmock${CMAKE_SHARED_LIBRARY_SUFFIX} 
		../gmock_src/libgmock_main${CMAKE_SHARED_LIBRARY_SUFFIX} 
		../gmock_src/gtest/libgtest${CMAKE_SHARED_LIBRARY_SUFFIX} 
		../gmock_src/gtest/libgtest_main${CMAKE_SHARED_LIBRARY_SUFFIX} 	
		${CATKIN_DEVEL_PREFIX}/lib/ &&
	cp -rf
		../gmock_src/include/gmock 
		../gmock_src/gtest/include/gtest 
		${CATKIN_DEVEL_PREFIX}/include/
)

cs_install()
cs_export(
  INCLUDE_DIRS ${CATKIN_DEVEL_PREFIX}/include
  CFG_EXTRAS gmock-extras.cmake)
