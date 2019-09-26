project(HookTest)

set(SRC
    HookTest.cpp
	)
source_group(TREE ${CMAKE_SOURCE_DIR} FILES ${SRC})

add_executable(HookTest ${SRC})
set_target_properties(HookTest PROPERTIES LINK_FLAGS "/SUBSYSTEM:WINDOWS /ENTRY:WinMainCRTStartup@@YAHXZ")
target_link_libraries(HookTest PRIVATE NktHookLib)

