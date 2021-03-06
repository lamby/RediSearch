# Find all the Python files with test_* in them
FILE(GLOB PY_TEST_FILES "test*.py")

FOREACH(n ${PY_TEST_FILES})
    GET_FILENAME_COMPONENT(test_name ${n} NAME_WE)
    ADD_TEST(NAME "PY_${test_name}"
        COMMAND "RLTest" "--module" "$<TARGET_FILE:redisearch>" "-t" "${test_name}.py" "--clear-logs"
        WORKING_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}")

    ADD_TEST(NAME "PY_${test_name}_SAFEMODE"
        COMMAND "RLTest" "--module" "$<TARGET_FILE:redisearch>" "-t" "${test_name}.py" "--module-args" "SAFEMODE" "--clear-logs"
        WORKING_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}")

    ADD_TEST(NAME "PY_${test_name}_FORKGC"
        COMMAND "RLTest" "--module" "$<TARGET_FILE:redisearch>" "-t" "${test_name}.py" "--module-args" "GC_POLICY FORK" "--clear-logs"
        WORKING_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}")

ENDFOREACH()
