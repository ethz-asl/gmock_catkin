cmake_minimum_required(VERSION 2.8.3)
project(gmock_catkin)

find_package(catkin_simple REQUIRED)

catkin_simple()

include(ExternalProject)

file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include)

ExternalProject_Add(gmock_src
  DOWNLOAD_COMMAND wget --retry-connrefused --waitretry=1 --timeout=12 --tries 10 https://googlemock.googlecode.com/files/gmock-1.7.0.zip
  UPDATE_COMMAND ""
  CONFIGURE_COMMAND cd ../gmock_src && cmake . -DCMAKE_CXX_FLAGS=-fPIC
  BUILD_COMMAND cd ../gmock_src && make -j8
  INSTALL_COMMAND cp ../gmock_src/libgmock.a ${CATKIN_DEVEL_PREFIX}/lib/ &&
		  cp ../gmock_src/libgmock_main.a ${CATKIN_DEVEL_PREFIX}/lib/ &&
		  cp ../gmock_src/gtest/libgtest.a ${CATKIN_DEVEL_PREFIX}/lib/ &&
		  cp ../gmock_src/gtest/libgtest_main.a ${CATKIN_DEVEL_PREFIX}/lib/ &&
		  cp -rf ../gmock_src/include/gmock ${CATKIN_DEVEL_PREFIX}/include/ &&
		  cp -rf ../gmock_src/gtest/include/gtest ${CATKIN_DEVEL_PREFIX}/include/
)

cs_install()
cs_export(
  INCLUDE_DIRS ${CATKIN_DEVEL_PREFIX}/include
  CFG_EXTRAS gmock-extras.cmake)
