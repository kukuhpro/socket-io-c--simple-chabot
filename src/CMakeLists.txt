cmake_minimum_required(VERSION 2.8)
project(socket.io-cpp-client-sample)

# socket.io
if(NOT SIO_DIR)
  message("SIO_DIR must be set.")
  return()
endif()
include_directories("${SIO_DIR}/include")
link_directories("${SIO_DIR}/lib/Release")
if (NOT ENABLE_SSL)
  list(APPEND extra_libs "sioclient")
else()
  list(APPEND extra_libs "sioclient_tls")
  find_package(OpenSSL)
  list(APPEND extra_libs ${OPENSSL_LIBRARIES})
endif()

# Boost
find_package(Boost REQUIRED COMPONENTS system date_time random)
include_directories(${Boost_INCLUDE_DIRS})
link_directories(${Boost_LIBRARY_DIRS})
list(APPEND extra_libs ${Boost_LIBRARIES})

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -W -Wall")
set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -O0")

if(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
  # MaxOSX
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")

else()
  # Linux or other UNIX like system
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
endif()

include_directories("helper")
include_directories("chatbot")

file(GLOB SOURCES
    ./helper/*.cpp
    ./helper/*.h
    ./chatbot/*.cpp
    ./chatbot/*.h
    ./*.cpp
)

add_executable(client ${SOURCES})
target_link_libraries(client ${extra_libs})
