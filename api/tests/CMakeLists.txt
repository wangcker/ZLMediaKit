﻿aux_source_directory(. TEST_SRC_LIST)
foreach(TEST_SRC ${TEST_SRC_LIST})
    STRING(REGEX REPLACE "^\\./|\\.c[a-zA-Z0-9_]*$" "" TEST_EXE_NAME ${TEST_SRC})
    message(STATUS "add c api tester:${TEST_EXE_NAME}")
    set(exe_name api_tester_${TEST_EXE_NAME})
    add_executable(${exe_name} ${TEST_SRC})
    if(USE_SOLUTION_FOLDERS)
        SET_PROPERTY(TARGET ${exe_name} PROPERTY FOLDER "api_test")
    endif()

	if(MSVC)
		set_target_properties(${exe_name} PROPERTIES COMPILE_FLAGS ${VS_FALGS} )
	endif()

    target_link_libraries(${exe_name} mk_api)
endforeach()












