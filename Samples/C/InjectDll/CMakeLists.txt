project(InjectDll)

set(SRC
    InjectDll.cpp
	)
source_group(TREE ${CMAKE_SOURCE_DIR} FILES ${SRC})
add_executable(InjectDll WIN32 ${SRC})
target_link_libraries(InjectDll PRIVATE NktHookLib)
set_target_properties(InjectDll PROPERTIES LINK_FLAGS "/SUBSYSTEM:CONSOLE")