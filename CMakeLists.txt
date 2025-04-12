cmake_minimum_required(VERSION 3.15)
project(WeaveCompiler)

set(CMAKE_CXX_STANDARD 17)

# Try to locate llvm-config
find_program(LLVM_CONFIG_EXECUTABLE NAMES llvm-config)

if (NOT LLVM_CONFIG_EXECUTABLE)
    message(FATAL_ERROR "llvm-config not found. Please install LLVM and make sure llvm-config is in your PATH.")
endif()

# Ask llvm-config for paths
execute_process(
    COMMAND ${LLVM_CONFIG_EXECUTABLE} --cmakedir
    OUTPUT_VARIABLE LLVM_CMAKE_DIR
    OUTPUT_STRIP_TRAILING_WHITESPACE
)

# Point to the correct LLVM config directory
set(LLVM_DIR "${LLVM_CMAKE_DIR}" CACHE PATH "LLVM cmake dir")

# Now find LLVM
find_package(LLVM REQUIRED CONFIG)

message(STATUS "Found LLVM ${LLVM_PACKAGE_VERSION}")
message(STATUS "Using LLVMConfig.cmake in: ${LLVM_DIR}")

include_directories(${LLVM_INCLUDE_DIRS})
add_definitions(${LLVM_DEFINITIONS})

add_executable(weave
    src/main.cpp
    # Add other source files here
)

llvm_map_components_to_libnames(llvm_libs support core irreader native)
target_link_libraries(weave ${llvm_libs})
