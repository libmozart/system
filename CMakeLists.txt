include_directories(.)

aux_source_directory(mozart++ SRCS)
aux_source_directory(src SRCS_IMPL)

add_library(mpp_system STATIC
        ${SRCS}
        ${SRCS_IMPL}
        )

target_link_libraries(mpp_system mpp_core)
target_link_libraries(mpp_system mpp_string)
set_target_properties(mpp_system PROPERTIES LINKER_LANGUAGE CXX)

## test and benchmark targets here
file(GLOB_RECURSE CPP_SRC_LIST tests/test-*.cpp)
foreach(v ${CPP_SRC_LIST})
    string(REGEX MATCH "tests/.*" relative_path ${v})
    string(REGEX REPLACE "tests/" "" target_name ${relative_path})
    string(REGEX REPLACE ".cpp" "" target_name ${target_name})

    add_executable(mpp_system-${target_name} ${v})
    target_link_libraries(mpp_system-${target_name} mpp_system)
    add_test(mpp_system-${target_name} mpp_system-${target_name})
endforeach()
