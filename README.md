# [gRPC Tutorials](#)
This repository contains my C++ snippets code with `gRPC` and `protobuf`.

## Protocol Buffers 


### Defining Your Protocol Format
To create your an address book, you'll need to create a `.proto` file, and populate it as follows:

```
// proto2 or proto3
syntax = "proto2";


//The .proto file starts with a package declaration, which helps to prevent naming conflicts between different projects
package tutorial; 



/* Message definitions: A message is just an aggregate containing a set of typed fields.

The " = 1", " = 2" markers on each element identify the unique "tag" that field uses in the binary encoding. 
Tag numbers 1-15 require one less byte to encode than higher numbers, so as an optimization you can decide to 
use those tags for the commonly used or repeated elements, leaving tags 16 and higher for less-commonly used optional elements
and should not be changed once your message type is in use.

They're the field numbers - they're used in the wire representation to identify which field is associated with a value. 
This means that renaming a field isn't a breaking change (in terms of wire format) and the names themselves don't have to be serialized.


Each field must be annotated with one of the following modifiers:
- optional: the field may or may not be set, If no value is set, a default value is used ( zero for numeric types, the empty string for strings, false for bools)
- repeated: Think of it as a dynamically sized arrays.
- required: a value for the field must be provided, otherwise the message will be considered "uninitialized". In debug mode, serializing an uninitialized message will cause an assertion failure and
parsing an uninitialized message will always fail by returning false.
*/

message Person 
{
  optional string name = 1;
  optional int32 id = 2;
  optional string email = 3;

  enum PhoneType 
  {
    MOBILE = 0;
    HOME = 1;
    WORK = 2;
  }

  message PhoneNumber 
  {
    optional string number = 1;
    optional PhoneType type = 2 [default = HOME];
  }

  repeated PhoneNumber phones = 4;
}

message AddressBook 
{
  repeated Person people = 1;
}
```

More on language guide: [proto2](https://developers.google.com/protocol-buffers/docs/proto) and [proto3](https://developers.google.com/protocol-buffers/docs/proto3).


### Compiling Protocol Buffers Files
After compiling `gRPC` the `protoc` will be built in 
`build/_deps/grpc-build/third_party/protobuf`

Now run the following:
```
SRC_DIR=../../../../../protobuf_messages
DST_DIR=../../../../../src
protoc --proto_path=$SRC_DIR --cpp_out=$DST_DIR  $SRC_DIR/addressbook.proto
```
It should create `addressbook.pb.cc` and `addressbook.pb.h` in `src` directory 

### Writing A Message

Refs: [1](https://github.com/protocolbuffers/protobuf/blob/main/src/README.md), [2](https://developers.google.com/protocol-buffers/docs/cpptutorial), [3](https://developers.google.com/protocol-buffers/docs/cpptutorial)

## gRPC

### Installation


```
cmake_minimum_required(VERSION 3.15)
project(gRPC_project)

include(FetchContent)
FetchContent_Declare
(
  gRPC
  GIT_REPOSITORY https://github.com/grpc/grpc
  #GIT_TAG        RELEASE_TAG_HERE  # e.g v1.28.0
)
set(FETCHCONTENT_QUIET OFF)
FetchContent_MakeAvailable(gRPC)

add_executable(main src/main.cpp)
target_link_libraries(main grpc++)
```

More about installation [here](https://github.com/grpc/grpc/tree/master/src/cpp)


Refs: [1](https://github.com/grpc/grpc/tree/master/src/cpp), [2](https://github.com/grpc/grpc/tree/master/examples/cpp/helloworld), [3](https://grpc.io/docs/languages/cpp/)



