find_package(GTest REQUIRED)
find_package(PCL REQUIRED COMPONENTS core features filters io segmentation surface)

set(SCO_TEST_SOURCE
    unit.cpp
    small-problems-unit.cpp
    solver-interface-unit.cpp
    solver-utils-unit.cpp
)

macro(add_gtest test_name test_file)
  add_executable(${test_name} ${test_file})
  target_compile_options(${test_name} PRIVATE -Wall -Wextra -Wsuggest-override -Wconversion -Wsign-conversion)
  target_compile_features(${test_name} PRIVATE cxx_std_11)
  target_compile_definitions(${test_name} PRIVATE DATA_DIR="${CMAKE_SOURCE_DIR}/test")
  target_include_directories(${test_name} PRIVATE
      "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/test>"
      ${GTEST_INCLUDE_DIRS})
  add_test(${test_name} ${test_name})
  add_dependencies(run_tests ${test_name})
endmacro()

add_gtest(${PROJECT_NAME}-test ${SCO_TEST_SOURCE})
target_link_libraries(${PROJECT_NAME}-test ${GTEST_BOTH_LIBRARIES} ${PROJECT_NAME})
if (osqp_FOUND)
    target_link_libraries(${PROJECT_NAME}-test osqp::osqpstatic)
endif()

add_gtest(${PROJECT_NAME}_planning_unit planning_unit.cpp)
target_link_libraries(${PROJECT_NAME}_planning_unit ${PROJECT_NAME} console_bridge ${Boost_SYSTEM_LIBRARY} ${Boost_PROGRAM_OPTIONS_LIBRARY})
target_include_directories(${PROJECT_NAME}_planning_unit PRIVATE ${PCL_INCLUDE_DIRS})

add_gtest(${PROJECT_NAME}_interface_unit interface_unit.cpp)
target_link_libraries(${PROJECT_NAME}_planning_unit ${PROJECT_NAME} console_bridge ${Boost_SYSTEM_LIBRARY} ${Boost_PROGRAM_OPTIONS_LIBRARY})
target_include_directories(${PROJECT_NAME}_planning_unit PRIVATE ${PCL_INCLUDE_DIRS})

add_gtest(${PROJECT_NAME}_costs_unit costs_unit.cpp)
target_link_libraries(${PROJECT_NAME}_planning_unit ${PROJECT_NAME} console_bridge ${Boost_SYSTEM_LIBRARY} ${Boost_PROGRAM_OPTIONS_LIBRARY})
target_include_directories(${PROJECT_NAME}_planning_unit PRIVATE ${PCL_INCLUDE_DIRS})

add_gtest(${PROJECT_NAME}_cast_cost_unit cast_cost_unit.cpp)
target_link_libraries(${PROJECT_NAME}_planning_unit ${PROJECT_NAME} console_bridge ${Boost_SYSTEM_LIBRARY} ${Boost_PROGRAM_OPTIONS_LIBRARY})
target_include_directories(${PROJECT_NAME}_planning_unit PRIVATE ${PCL_INCLUDE_DIRS})

add_gtest(${PROJECT_NAME}_cast_cost_world_unit cast_cost_world_unit.cpp)
target_link_libraries(${PROJECT_NAME}_planning_unit ${PROJECT_NAME} console_bridge ${Boost_SYSTEM_LIBRARY} ${Boost_PROGRAM_OPTIONS_LIBRARY})
target_include_directories(${PROJECT_NAME}_planning_unit PRIVATE ${PCL_INCLUDE_DIRS})

add_gtest(${PROJECT_NAME}_cast_cost_attached_unit cast_cost_attached_unit.cpp)
target_link_libraries(${PROJECT_NAME}_planning_unit ${PROJECT_NAME} console_bridge ${Boost_SYSTEM_LIBRARY} ${Boost_PROGRAM_OPTIONS_LIBRARY})
target_include_directories(${PROJECT_NAME}_planning_unit PRIVATE ${PCL_INCLUDE_DIRS})

add_gtest(${PROJECT_NAME}_cast_cost_octomap_unit cast_cost_octomap_unit.cpp)
target_link_libraries(${PROJECT_NAME}_planning_unit ${PROJECT_NAME} console_bridge ${Boost_SYSTEM_LIBRARY} ${Boost_PROGRAM_OPTIONS_LIBRARY})
target_include_directories(${PROJECT_NAME}_planning_unit PRIVATE ${PCL_INCLUDE_DIRS})
