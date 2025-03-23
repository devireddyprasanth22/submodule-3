# submodule-1/CMakeLists.txt
cmake_minimum_required(VERSION 3.10)
project(submodule-3)

enable_testing()

# Add submodule-1.1
add_subdirectory(submodule-3.1)
add_subdirectory(submodule-3.2)

# Create a library for submodule-1
add_library(submodule3_lib submodule3.c)
add_executable(submodule3 submodule3.c)

# Add Google Test
find_package(GTest REQUIRED)
include_directories(${GTEST_INCLUDE_DIRS})

# Link submodule-3 with submodule-3.1 and 3.2
target_link_libraries(submodule3_lib
    PRIVATE
    submodule31_lib
    submodule32_lib)

# Create test executable
add_executable(submodule3_tests test-submodule3.cpp)

# Link test executable with required libraries
target_link_libraries(submodule3_tests
    PRIVATE
    GTest::GTest
    GTest::Main
    pthread
    submodule3_lib)

# Register test
add_test(NAME submodule3_test COMMAND submodule3_tests)

# Make the library target available to parent projects
set_target_properties(submodule3_lib PROPERTIES EXPORT_NAME submodule3_lib)
