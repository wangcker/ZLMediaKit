﻿execute_process(COMMAND cp ${CMAKE_CURRENT_SOURCE_DIR}/default.pem ${EXECUTABLE_OUTPUT_PATH}/)
aux_source_directory(. TEST_SRC_LIST)

foreach (TEST_SRC ${TEST_SRC_LIST})
    get_filename_component(TEST_EXE_NAME ${TEST_SRC} NAME_WE)

    if (NOT ENABLE_WEBRTC)
        # 暂时过滤掉依赖 WebRTC 的测试模块
        if ("${TEST_EXE_NAME}" MATCHES "test_rtcp_nack")
            continue()
        endif ()
    endif ()

    message(STATUS "add test:${TEST_EXE_NAME}")
    add_executable(${TEST_EXE_NAME} ${TEST_SRC})

    if(USE_SOLUTION_FOLDERS)
        SET_PROPERTY(TARGET ${TEST_EXE_NAME} PROPERTY FOLDER "test")
    endif ()

    if (MSVC)
        set_target_properties(${TEST_EXE_NAME} PROPERTIES COMPILE_FLAGS ${VS_FALGS})
    endif ()

    if (CMAKE_SYSTEM_NAME MATCHES "Linux")
        target_link_libraries(${TEST_EXE_NAME} -Wl,--start-group ${LINK_LIB_LIST} -Wl,--end-group)
    else ()
        target_link_libraries(${TEST_EXE_NAME} ${LINK_LIB_LIST})
    endif ()
endforeach ()












