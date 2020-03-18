cmake_minimum_required(VERSION 3.5)

project(gzip_cpp)

# Allow support for C++11
if (CMAKE_VERSION VERSION_LESS "3.1")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
else ()
    set(CMAKE_CXX_STANDARD 11)
endif ()

set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -DDEBUG")

find_package(ZLIB REQUIRED)
find_package(GTest REQUIRED)

include_directories(
        "include"
        ${GTEST_INCLUDE_DIRS}
        ${ZLIB_INCLUDE_DIRS}
)

file(GLOB GZIP_SRCS "src/*.cc")
add_library(gzip_cpp ${GZIP_SRCS})

target_link_libraries(gzip_cpp ${ZLIB_LIBRARIES} pthread)

file(GLOB GZIP_TEST_SRCS "test/*.cc")

add_executable(gzip_cpp_test EXCLUDE_FROM_ALL ${GZIP_TEST_SRCS})
target_link_libraries(gzip_cpp_test gzip_cpp ${GTEST_BOTH_LIBRARIES})

# Test can be built using 'make check'
add_custom_target(check)
add_dependencies(check gzip_cpp_test)

file(GLOB INCLUDE_FILES "include/*.h")
install(FILES ${INCLUDE_FILES} DESTINATION "include/gzip_cpp")
install(TARGETS gzip_cpp DESTINATION "lib")
