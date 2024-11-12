cmake_minimum_required(VERSION 3.10)
project(BisonFlexExample LANGUAGES CXX)

# Set C++ standard
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# Find Flex and Bison
find_package(FLEX REQUIRED)
find_package(BISON REQUIRED)

# Define Bison and Flex output files
bison_target(Parser parser.yy ${CMAKE_CURRENT_BINARY_DIR}/parser.cpp
    DEFINES_FILE ${CMAKE_CURRENT_BINARY_DIR}/parser.hh
)

flex_target(Scanner scanner.ll ${CMAKE_CURRENT_BINARY_DIR}/scanner.cpp)
add_flex_bison_dependency(Scanner Parser)

# Add the main executable
add_executable(${PROJECT_NAME} 
    calc.cc
    driver.cc 
    ${CMAKE_BINARY_DIR}/parser.cpp
    ${CMAKE_BINARY_DIR}/scanner.cpp
    ${BISON_Parser_OUTPUTS}
)

# Include directories for generated headers and project headers
target_include_directories(${PROJECT_NAME} PRIVATE 
    ${CMAKE_BINARY_DIR} 
    ${CMAKE_SOURCE_DIR}
)

