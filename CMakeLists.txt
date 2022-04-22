cmake_minimum_required(VERSION 3.5)
project(robotoc_vendor)

# Default to C++17 to avoid alignment errors with -march=native
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 17)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()
option(OPTIMIZE_FOR_NATIVE "Enable -march=native" OFF)

# find dependencies
find_package(ament_cmake REQUIRED)

set(robotoc_SRCDIR ${CMAKE_CURRENT_BINARY_DIR}/robotoc_src)
set(robotoc_DESTDIR ${CMAKE_CURRENT_BINARY_DIR}/robotoc_install)

include(FetchContent)
FetchContent_GetProperties(robotoc)
if(NOT robotoc_POPULATED)
  FetchContent_Populate(
    robotoc
    GIT_REPOSITORY https://github.com/mayataka/robotoc
    GIT_TAG master
    GIT_SHALLOW TRUE
    SOURCE_DIR ${robotoc_SRCDIR})
endif()

include(ExternalProject)
ExternalProject_Add(
  robotoc
  SOURCE_DIR ${robotoc_SRCDIR}
  BUILD_IN_SOURCE TRUE
  INSTALL_DIR ${robotoc_DESTDIR}
  CONFIGURE_COMMAND ${CMAKE_COMMAND} -DCMAKE_INSTALL_PREFIX=<INSTALL_DIR> -DCMAKE_CXX_STANDARD=${CMAKE_CXX_STANDARD} -DBUILD_TYPE=${CMAKE_BUILD_TYPE} -DOPTIMIZE_FOR_NATIVE=${OPTIMIZE_FOR_NATIVE} -DBUILD_PYTHONE_INTERFACE=OFF -DCMAKE_POSITION_INDEPENDENT_CODE=ON 
)

ament_export_include_directories(include)
ament_export_libraries(robotoc)
ament_export_dependencies(robotoc pinocchio)

install(
  DIRECTORY ${robotoc_DESTDIR}/
  DESTINATION ${CMAKE_INSTALL_PREFIX}
)

ament_package()