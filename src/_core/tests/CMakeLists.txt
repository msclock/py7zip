find_package(GTest CONFIG REQUIRED)

include(GoogleTest)

file(GLOB files "*.cpp")

foreach(_file ${files})
  get_filename_component(file_basename ${_file} NAME_WE)
  add_executable(${file_basename} ${_file})
  target_link_libraries(${file_basename} PRIVATE GTest::gtest_main
                                                 _core_internal)
  target_compile_definitions(${file_basename}
                             PRIVATE TESTS_DIR="${CMAKE_CURRENT_SOURCE_DIR}")
  warn_target(${file_basename})
  harden_target(${file_basename})
  sanitize_target(${file_basename})
  gtest_discover_tests(${file_basename})
endforeach()
