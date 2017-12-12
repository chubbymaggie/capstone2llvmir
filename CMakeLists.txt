
cmake_minimum_required(VERSION 3.6)

project(capstone2llvmir CXX)

# Set the default build type to 'Release'
if (NOT CMAKE_BUILD_TYPE)	
	set(default_build_type "Release")
	message(STATUS "Setting build type to '${default_build_type}' as none was specified.")
	set(CMAKE_BUILD_TYPE "${default_build_type}" CACHE STRING "Choose the type of build." FORCE)
endif()

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_INSTALL_RPATH "${CMAKE_INSTALL_PREFIX}/lib")

option(CAPSTONE2LLVMIR_DOC "Build public API documentation (requires Doxygen)." OFF)
option(CAPSTONE2LLVMIR_TOOLS "Build tools." OFF)
option(CAPSTONE2LLVMIR_TESTS "Build tests." OFF)

# Configure Capstone build.
set(CAPSTONE_BUILD_SHARED OFF CACHE BOOL "disable capstone build shared")
if(MSVC)
	set(CAPSTONE_BUILD_STATIC_RUNTIME OFF CACHE BOOL "disable capstone static runtime")
endif()

# Configure Keystone build.
if(MSVC)
	set(KEYSTONE_BUILD_STATIC_RUNTIME OFF CACHE BOOL "disable keystone static runtime")

	# Keystone requires python3, so we first check whether python3 is installed as
	# "python3", and if not, we suppose that `python` runs python3.
	# Python is used to generate LLVMBuild.cmake file. Python2 generates this file
	# with UNIX paths, which do not work. Python3 generates proper Windows paths.
	find_program(PYTHON_EXECUTABLE "python3")
	if(NOT PYTHON_EXECUTABLE)
		find_program(PYTHON_EXECUTABLE "python")
	endif()
endif()

add_subdirectory(deps)
if(CAPSTONE2LLVMIR_DOC)
	add_subdirectory(doc)
endif()
add_subdirectory(src)
if(CAPSTONE2LLVMIR_TESTS)
	add_subdirectory(tests)
endif()
