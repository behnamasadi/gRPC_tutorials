cmake_minimum_required(VERSION 3.15)
project(grpc_project)

include(FetchContent)
FetchContent_Declare(
  gRPC
  GIT_REPOSITORY https://github.com/grpc/grpc
#  GIT_TAG        RELEASE_TAG_HERE  # e.g v1.28.0
)
#set(FETCHCONTENT_QUIET OFF)
FetchContent_MakeAvailable(gRPC)




IF(IS_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/src/sandbox")
        ADD_SUBDIRECTORY("${CMAKE_CURRENT_SOURCE_DIR}/src/sandbox")
ENDIF()


find_package(protobuf CONFIG REQUIRED)

if(protobuf_VERBOSE)
  message(STATUS "Using Protocol Buffers ${protobuf_VERSION}")
endif()


MESSAGE(${CMAKE_CURRENT_SOURCE_DIR})
IF(EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/src/addressbook.pb.h")
	add_executable(writing_message src/writing_message.cpp   src/addressbook.pb.cc)
	target_link_libraries(writing_message)
ENDIF()



add_executable(main src/main.cpp)
target_link_libraries(main grpc++)
