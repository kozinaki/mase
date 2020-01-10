cmake_minimum_required(VERSION 3.15)
project(mase)

set(CMAKE_CXX_STANDARD 14)
set(BOOST_ROOT "~/dev/boost/")
set(BOOSTROOT "~/dev/boost/")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -pthread")

find_package(Boost 1.72.0)
IF (Boost_FOUND)
    include_directories(${Boost_INCLUDE_DIR})
endif()

include_directories(~/dev/openssl/include)
set(myDesiredRootForHeaderFileSearch ${CMAKE_SOURCE_DIR}/core)
file(GLOB my_include_directories ${myDesiredRootForHeaderFileSearch})
message(${my_include_directories})
include_directories(${my_include_directories})

file (GLOB_RECURSE CPP_FILES *.cpp)
set (SOURCE_FILES ${CPP_FILES})

add_executable(mase ${SOURCE_FILES})