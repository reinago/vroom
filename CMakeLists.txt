cmake_minimum_required(VERSION 3.12 FATAL_ERROR)

project(vroom
  LANGUAGES C CXX)

set (CMAKE_CXX_STANDARD 11)
add_compile_definitions(VROOM_STANDALONE)

add_subdirectory("src/mio")

file(GLOB source_files RELATIVE "${CMAKE_CURRENT_SOURCE_DIR}" "src/delimited_index.cc")
file(GLOB header_files RELATIVE "${CMAKE_CURRENT_SOURCE_DIR}" "src/*.h")
set(public_header_files ${header_files})

add_library(${PROJECT_NAME} STATIC ${source_files} ${header_files})
target_include_directories(${PROJECT_NAME}
  PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}> $<INSTALL_INTERFACE:include>)
target_link_libraries(${PROJECT_NAME} PUBLIC mio::mio)
set_target_properties(${PROJECT_NAME} PROPERTIES POSITION_INDEPENDENT_CODE ON)

install(TARGETS ${PROJECT_NAME} DESTINATION "lib")
install(FILES ${public_header_files} DESTINATION "include")
