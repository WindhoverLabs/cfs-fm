include(CTest)
find_program( MEMORYCHECK_COMMAND valgrind )
find_package(Threads)
set( MEMORYCHECK_COMMAND_OPTIONS "--trace-children=yes --leak-check=full" )

# Set include directories
include_directories(
	../mission_inc
	../platform_inc
	$ENV{CFE_DIR}/fsw/mission_inc
	$ENV{CFE_DIR}/fsw/platform_inc/cpu1
	${CMAKE_CURRENT_SOURCE_DIR}
	${CFE_INC_DIRS}
	${OSAL_INC_DIRS}
	${UTF_INCS}
	${APP_INC_DIRS})

add_definitions(-DCFE_LINUX)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -g -O0 -Wformat=0 -Wno-pointer-to-int-cast -Wno-int-to-pointer-cast -fprofile-arcs -ftest-coverage")

# Add the executables
add_executable(FM-UT 
		${CMAKE_CURRENT_SOURCE_DIR}/fm_utest_app.c
		${CMAKE_CURRENT_SOURCE_DIR}/fm_utest_child.c
		${CMAKE_CURRENT_SOURCE_DIR}/fm_utest_cmds.c
		${CMAKE_CURRENT_SOURCE_DIR}/fm_utest_tbl.c
		${CMAKE_CURRENT_SOURCE_DIR}/fm_utest_utils.c
		${CMAKE_CURRENT_SOURCE_DIR}/../tables/fm_freespace.c
		${CMAKE_CURRENT_SOURCE_DIR}/fm_utest.c
		${CMAKE_CURRENT_SOURCE_DIR}/../../../cfs_lib/fsw/src/cfs_utils.c
		${APP_SRC}
		${UTF_SRC})
target_link_libraries(FM-UT -fprofile-arcs gcov ${CMAKE_THREAD_LIBS_INIT})

# We need to both run the test and compare the as-run with the expected output
# with a diff command. 
add_test(NAME fm-ut COMMAND FM-UT)

# Copy input files to build directory
configure_file(${CMAKE_CURRENT_SOURCE_DIR}/sbsim.in ${CMAKE_CURRENT_BINARY_DIR} COPYONLY)
    
# Create ram disk mount point for unit tests
file(MAKE_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/ram)
    
# Build tables for the tests
add_subdirectory(../tables cf/apps)


