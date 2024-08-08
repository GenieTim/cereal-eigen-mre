cmake_minimum_required(VERSION 3.5.0)
project(main VERSION 0.1.0 LANGUAGES C CXX)

# specify the C++ standard
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED True)

if(CMAKE_COMPILER_IS_GNUCXX)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g -pedantic -Wall -Wextra -Wcast-align -Wcast-qual -Wctor-dtor-privacy -Wdisabled-optimization -Wformat=2 -Winit-self -Wmissing-declarations -Wmissing-include-dirs -Wold-style-cast -Woverloaded-virtual -Wredundant-decls -Wshadow -Wsign-conversion -Wsign-promo -Wstrict-overflow=5 -Wswitch-default -Wundef -Wno-unused")
endif()

# setup dependencies
list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_LIST_DIR}/vendor")
include(${CMAKE_CURRENT_LIST_DIR}/vendor/eigen.CMakeLists.cmake)
include(${CMAKE_CURRENT_LIST_DIR}/vendor/cereal.CMakeLists.cmake)

# setup main
add_executable(main main.cpp)

add_dependencies(main cerealLib)

# additional dependency
find_package(OpenMP)
if (OpenMP_CXX_FOUND)
	include_directories(${OpenMP_CXX_INCLUDE_DIRS})
	target_link_libraries(main PUBLIC OpenMP::OpenMP_CXX)
	message(STATUS "Found OpenMP")
else()
	message(AUTHOR_WARNING "Did not find OpenMP")
endif()

include_directories(${cereal_INCLUDE_DIRS})
target_link_libraries(main PUBLIC Eigen3::Eigen)
