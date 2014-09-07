set(THRIFT_FILE ${FB303_DIR}/if/fb303.thrift)
set(THRIFT_OUTDIR ${FB303_DIR}/cpp/gen-cpp)

find_program(THRIFT_BIN thrift /bin /usr/bin /usr/local/bin)

if(THRIFT_BIN)
    if (EXISTS ${THRIFT_FILE})
        execute_process(COMMAND ${THRIFT_BIN} -r -gen cpp:pure_enums -o ${FB303_DIR}/cpp ${THRIFT_FILE}
		                RESULT_VARIABLE result)
        set_directory_properties(${CMAKE_CURRENT_LIST_DIR} ADDITIONAL_MAKE_CLEAN_FILES ${THRIFT_OUTDIR})
    else (EXISTS ${THRIFT_FILE})
        message(SEND_ERROR "${THRIFT_FILE} does not exists!")
    endif (EXISTS ${THRIFT_FILE})

else (THRIFT_BIN)
    message(SEND_ERROR "Cannot find thrift in your path.")
endif (THRIFT_BIN)

include_directories(${CONTRIB_DIR})

file(COPY FacebookBase.h
	DESTINATION include/fb303
	)
	
include_directories(${CMAKE_CURRENT_LIST_DIR}/include)

add_library(libfb303
    ${THRIFT_OUTDIR}/FacebookService.cpp
	${THRIFT_OUTDIR}/fb303_constants.cpp
	${THRIFT_OUTDIR}/fb303_types.cpp
	FacebookBase.cpp
	ServiceTracker.cpp
)