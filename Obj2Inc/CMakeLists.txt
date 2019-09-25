project(Obj2Inc)

set(SRC
    Obj2Inc.cpp
	)
source_group(TREE ${CMAKE_SOURCE_DIR} FILES Obj2Inc.cpp)
add_executable(Obj2Inc ${SRC})
target_link_libraries(Obj2Inc PRIVATE )