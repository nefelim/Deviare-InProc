project(Helpers LANGUAGES ASM)

enable_language(ASM_MASM)
if(CMAKE_SIZEOF_VOID_P EQUAL 8)
    set(ASM
        Helpers_x64.asm
        RelocatableCode_x64.asm
    )
else()
    set(ASM
        Helpers_x86.asm
        RelocatableCode_x86.asm
    )
endif()

set_source_files_properties(${ASM} PROPERTIES COMPILE_FLAGS "/c /nologo /safeseh")

source_group(TREE ${CMAKE_SOURCE_DIR} FILES ${ASM})

add_library(Helpers STATIC ${ASM})
add_dependencies(Helpers Obj2Inc)

function(BuildAsmFile asm_cmd filename)
    add_custom_command(TARGET Helpers
        POST_BUILD COMMAND ${asm_cmd} /c /nologo /safeseh /Fo$(IntDir)${filename}.obj ${CMAKE_CURRENT_SOURCE_DIR}/${filename}.asm
        MAIN_DEPENDENCY ${CMAKE_CURRENT_SOURCE_DIR}/${filename}.asm
        VERBATIM)
endfunction(BuildAsmFile)

function(GenInlineFile filename var)
    add_custom_command(TARGET Helpers
        POST_BUILD COMMAND ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/Obj2Inc.exe $(IntDir)${filename}.obj /out ${GEN}/${filename}.inl /var ${var}
        MAIN_DEPENDENCY ${CMAKE_CURRENT_SOURCE_DIR}/${filename}.asm
        VERBATIM)
endfunction(GenInlineFile)

if(CMAKE_SIZEOF_VOID_P EQUAL 8)
    BuildAsmFile("ml.exe" "RelocatableCode_x86")
    BuildAsmFile("ml.exe" "Helpers_x86")
    GenInlineFile("RelocatableCode_x86" "RelocatableCodeX86")
    GenInlineFile("RelocatableCode_x64" "RelocatableCodeX64")
endif()