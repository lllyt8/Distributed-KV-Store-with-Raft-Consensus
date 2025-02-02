# Set the minimum cmake version and project name
cmake_minimum_required(VERSION 3.22)
project(KVRaftCpp)






set(CMAKE_CXX_STANDARD 20)
# Generate debug version, you can debug gdb
set(CMAKE_BUILD_TYPE "Debug")

# Set the path to the output of the project executable
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
# Set the path to the output of project library files
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

# Set the project compilation header file search path -I
# There may be a path contamination problem that needs to be further resolved
include_directories(${PROJECT_SOURCE_DIR}/src/common/include)
include_directories(${PROJECT_SOURCE_DIR}/src/fiber/include)
include_directories(${PROJECT_SOURCE_DIR}/src/rpc/include)
include_directories(${PROJECT_SOURCE_DIR}/example)
include_directories(${PROJECT_SOURCE_DIR}/src/raftCore/include)
include_directories(${PROJECT_SOURCE_DIR}/src/raftRpcPro/include)
include_directories(${PROJECT_SOURCE_DIR}/src/raftClerk/include)
include_directories(${PROJECT_SOURCE_DIR}/src/skipList/include)


# Set project library file search path -L
link_directories(${PROJECT_SOURCE_DIR}/lib)

# src contains all the relevant code
add_subdirectory(src)
# example contains sample code to use
add_subdirectory(example)

add_library(skip_list_on_raft STATIC  ${src_rpc} ${src_fiber} ${rpc_example} ${raftsource} ${src_raftCore}  ${src_raftRpcPro})
target_link_libraries(skip_list_on_raft muduo_net muduo_base pthread dl)
# Add formatting target start
# from : https://blog.csdn.net/guotianqing/article/details/121661067

add_custom_target(format
COMMAND bash ${PROJECT_SOURCE_DIR}/format.sh
WORKING_DIRECTORY ${PROJECT_SOURCE_DIR}
COMMAND echo "format done!"
)


# Add formatting target end
