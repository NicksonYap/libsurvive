
add_executable(survive_tests
        main.c
        reproject.c
        kalman.c rotate_angvel.c watchman.c ../driver_vive.c)

add_definitions(-DDEBUG_WATCHMAN)

target_link_libraries(survive_tests survive)

add_test(NAME survive_tests COMMAND survive_tests)
set_target_properties(survive_tests PROPERTIES FOLDER "tests")

add_executable(test_replays test_replays.c)
set_target_properties(test_replays PROPERTIES FOLDER "tests")
target_link_libraries(test_replays survive)

add_test(NAME lh1_test_cal COMMAND ./test_replays     ${CMAKE_CURRENT_BINARY_DIR}/libsurvive-extras-data/tests/lh1_test_cal.rec.gz)
add_test(NAME lh2_test_cal COMMAND ./test_replays     ${CMAKE_CURRENT_BINARY_DIR}/libsurvive-extras-data/tests/lh2_test_cal.rec.gz)
add_test(NAME lh2_test_cal_usb COMMAND ./test_replays ${CMAKE_CURRENT_BINARY_DIR}/libsurvive-extras-data/tests/lh2_test_cal.pcap.gz)

include(ExternalProject)

ExternalProject_Add(libsurvive-extras-data
        GIT_REPOSITORY https://github.com/jdavidberger/libsurvive-extras-data.git
        BUILD_COMMAND ""
        INSTALL_COMMAND ""
        CONFIGURE_COMMAND ""
        SOURCE_DIR ${CMAKE_CURRENT_BINARY_DIR}/libsurvive-extras-data
        EXCLUDE_FROM_ALL True
        )

add_custom_target(run_all_tests COMMAND ${CMAKE_CTEST_COMMAND} DEPENDS test_replays libsurvive-extras-data survive_tests survive_plugins)
