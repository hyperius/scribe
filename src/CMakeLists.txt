include_directories(contrib/fb303/cpp/include)

add_executable(router
	scribe_server.cpp
)

target_link_libraries(router ${Boost_LIBRARIES})
target_link_libraries(router libfb303)
