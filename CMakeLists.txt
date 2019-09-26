cmake_minimum_required(VERSION 3.12)

option(DEVIARE_BUILD_SAMPLES "Build deviar's samples." ON)

project(DeviareInProc LANGUAGES CXX)

if (NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Debug CACHE STRING "Choose the type of build, options are: Debug Release" FORCE)
endif ()

if(CMAKE_SIZEOF_VOID_P EQUAL 8)
    set(PLATFORM_STR "x64")
else()
    set(PLATFORM_STR "x86")
endif()

set(CMAKE_CXX_STANDARD 14)

if(MSVC)
    set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE}")
    set(CMAKE_CXX_FLAGS_MINSIZEREL "${CMAKE_CXX_FLAGS_MINSIZEREL}")
    set(CMAKE_CXX_FLAGS_RELWITHDEBINFO "${CMAKE_CXX_FLAGS_RELWITHDEBINFO}")
    set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG}")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /W3 /MP /GS- /Gd /FC /GR- -DUNICODE -D_UNICODE -D_WIN32_WINNT=NTDDI_WIN8")

    set(CMAKE_C_FLAGS_RELEASE "${CMAKE_C_FLAGS_RELEASE}")
    set(CMAKE_C_FLAGS_MINSIZEREL "${CMAKE_C_FLAGS_MINSIZEREL}")
    set(CMAKE_C_FLAGS_RELWITHDEBINFO "${CMAKE_C_FLAGS_RELWITHDEBINFO}")
    set(CMAKE_C_FLAGS_DEBUG "${CMAKE_C_FLAGS_DEBUG}")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} /W3 /MP -DUNICODE -D_UNICODE -D_WIN32_WINNT=NTDDI_WIN8")

    add_compile_options(
        $<$<CONFIG:>:/MT>
        $<$<CONFIG:Debug>:/MTd>
        $<$<CONFIG:Release>:/MT>
    )

endif()

string(REGEX REPLACE "/EHsc " "" CMAKE_CXX_FLAGS ${CMAKE_CXX_FLAGS})
set(ADDITIONAL_FLAGS_FILE "${CMAKE_CURRENT_BINARY_DIR}${CMAKE_FILES_DIRECTORY}/flags.h")
file(WRITE ${ADDITIONAL_FLAGS_FILE} "#pragma runtime_checks(\"suc\", off)")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /FI${ADDITIONAL_FLAGS_FILE}") # include file to disable RTC

set_property(GLOBAL PROPERTY USE_FOLDERS ON)
enable_testing()

set(CMAKE_INSTALL_PREFIX "${CMAKE_SOURCE_DIR}/../bin/${PLATFORM_STR}")

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_INSTALL_PREFIX})
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_RELEASE ${CMAKE_INSTALL_PREFIX})
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_MINSIZEREL ${CMAKE_INSTALL_PREFIX})
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_RELWITHDEBINFO ${CMAKE_INSTALL_PREFIX})
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_DEBUG ${CMAKE_INSTALL_PREFIX})

file(MAKE_DIRECTORY gen)
set(GEN "${CMAKE_SOURCE_DIR}/../gen")

add_library(DeviareInProc INTERFACE)
target_include_directories(DeviareInProc
    INTERFACE
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}/${PROJECT_NAME}>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
)

#add_subdirectory(DeviareLiteCOM)
#add_subdirectory(DeviareLiteInterop)
target_include_directories(DeviareInProc INTERFACE Include)
add_subdirectory(NktHookLib)
add_subdirectory(Obj2Inc)

# check if building as a stand-alone project
#making sure we can build standalone under windows
get_filename_component(CURRENT_SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR} ABSOLUTE)
get_filename_component(SOURCE_DIR ${CMAKE_SOURCE_DIR} ABSOLUTE)

if(${CURRENT_SOURCE_DIR} STREQUAL ${SOURCE_DIR} AND DEVIARE_BUILD_SAMPLES)
    file(GLOB_RECURSE DEVIARE_HEADERS ${CMAKE_CURRENT_SOURCE_DIR}/Include/*.h)
    add_library(deviare-headers STATIC ${DEVIARE_HEADERS})
    set_target_properties(deviare-headers PROPERTIES LINKER_LANGUAGE CXX)
    set_target_properties(deviare-headers PROPERTIES FOLDER Include)
    add_subdirectory(Samples)
endif()