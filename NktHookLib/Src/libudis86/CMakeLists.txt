project(libudis86 LANGUAGES CXX C)

set(SRC
	amalgamation.c
	GetInstructionLength.cpp 
    )

file(GLOB_RECURSE HEADERS "*.h")

source_group(TREE ${CMAKE_SOURCE_DIR} FILES ${SRC})
source_group(TREE ${CMAKE_SOURCE_DIR} FILES ${HEADERS})

add_library(libudis86 STATIC ${SRC} ${HEADERS})
target_include_directories(libudis86 PRIVATE tables)
target_link_libraries(libudis86 PRIVATE DeviareInProc)