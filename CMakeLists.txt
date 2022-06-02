cmake_minimum_required(VERSION 3.15)
project(grpc_project)

include(FetchContent)
FetchContent_Declare(
  gRPC
  GIT_REPOSITORY https://github.com/grpc/grpc
#  GIT_TAG        RELEASE_TAG_HERE  # e.g v1.28.0
)
set(FETCHCONTENT_QUIET OFF)
FetchContent_MakeAvailable(gRPC)

add_executable(main src/main.cpp)
target_link_libraries(main grpc++)
