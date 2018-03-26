GET_DIRECTORY_PROPERTY( HAS_PARENT PARENT_DIRECT)

set(ExternalBoostRoot ${CMAKE_SOURCE_DIR}/external/boost-1.66.0)
set(BOOTSTRAP_CMD ${ExternalBoostRoot}/bootstrap.bat)
set(BOOTSTRAP_ARGS gcc)
set(B2_CMD ${ExternalBoostRoot}/b2.exe)
set(B2_ARGS "--build-type=minimal --with-program_options --toolset=gcc --prefix=C:\\boost_1_66_0 install")

if(EXISTS ${BOOTSTRAP_CMD})
    message("Execute command: ${BOOTSTRAP_CMD} ${BOOTSTRAP_ARGS}")
    execute_process(COMMAND cmd  "${BOOTSTRAP_CMD}"
                    WORKING_DIRECTORY ${ExternalBoostRoot}
                    RESULT_VARIABLE BS_RESULT
                    OUTPUT_VARIABLE BS_OUTPUT
                    ERROR_VARIABLE BS_ERROR)

    if(EXISTS ${B2_CMD})
        message("Execute command: ${B2_CMD} ${B2_ARGS}")
        execute_process(COMMAND cmd "${B2_CMD} ${B2_ARGS}"
                        WORKING_DIRECTORY ${ExternalBoostRoot}
                        RESULT_VARIABLE BS_RESULT
                        OUTPUT_VARIABLE BS_OUTPUT
                        ERROR_VARIABLE BS_ERROR)
    endif()
endif()