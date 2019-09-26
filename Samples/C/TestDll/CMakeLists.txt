project(TestDll)

set(SRC
    TestDll.cpp
	)
source_group(TREE ${CMAKE_SOURCE_DIR} FILES ${SRC})
add_library(TestDll SHARED ${SRC})
target_link_libraries(TestDll PRIVATE )