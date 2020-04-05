enable_testing()


# Fetch catch2
FetchContent_Declare(
	catch2
	GIT_REPOSITORY https://github.com/catchorg/Catch2.git
)

FetchContent_GetProperties(catch2)
if(NOT catch2_POPULATED)
  FetchContent_Populate(catch2)
  message(STATUS "${catch2_SOURCE_DIR}")
  message(STATUS "${catch2_BINARY_DIR}")
  add_subdirectory(${catch2_SOURCE_DIR} ${catch2_BINARY_DIR})
endif()


# Find and add tests
file( GLOB TEST_SOURCES test_*.cxx  )

foreach(test_source ${TEST_SOURCES})
    get_filename_component(test_name ${test_source} NAME_WE)
    add_executable(${test_name} ${test_source} )
    target_link_libraries(${test_name}
        Catch2::Catch2
    )
   add_test(NAME ${test_name} COMMAND ${test_name})
endforeach()
