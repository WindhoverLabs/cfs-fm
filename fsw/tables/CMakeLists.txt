# Build tables for the tests
SET(CMAKE_CXX_ARCHIVE_CREATE "<CMAKE_AR> <OBJECTS> ${CMAKE_CURRENT_BINARY_DIR}")
SET(CMAKE_C_ARCHIVE_CREATE "<CMAKE_AR> <OBJECTS> ${CMAKE_CURRENT_BINARY_DIR}")
add_library(fm_freespace STATIC fm_freespace.c)

set_target_properties(fm_freespace PROPERTIES PREFIX "" SUFFIX ".tbl")
