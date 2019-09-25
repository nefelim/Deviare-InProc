project(NktHookLib LANGUAGES CXX)

set(SRC
    Src/DynamicApiFinder.cpp
    Src/DynamicNtApi.cpp
    Src/HookEntry.cpp
    Src/NktHookLib.cpp
    Src/NktHookLibHelpers.cpp
    Src/NtSysCallGen.cpp
    Src/ProcessEntry.cpp
    Src/ProcessInjection.cpp
    Src/RelocatableCode.cpp
    Src/ThreadSuspend.cpp
    Src/WaitableObjects.cpp
    Src/Wow64.cpp
    )

file(GLOB_RECURSE HEADERS "*.h")

source_group(TREE ${CMAKE_SOURCE_DIR} FILES ${SRC})
source_group(TREE ${CMAKE_SOURCE_DIR} FILES ${HEADERS})
source_group(TREE ${CMAKE_SOURCE_DIR} FILES ${gen})

add_library(NktHookLib STATIC ${SRC} ${HEADERS})
target_include_directories(NktHookLib PRIVATE
  $<BUILD_INTERFACE:${GEN}>
)

target_link_libraries(NktHookLib PRIVATE Helpers DeviareInProc)

add_subdirectory(Src/Asm)
add_subdirectory(Src/libudis86)