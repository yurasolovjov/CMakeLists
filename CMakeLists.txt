cmake_minimum_required (VERSION 2.8)
project (lua)  # project here actually means solution in premake

if(WIN32)
        add_definitions( -D_CRT_SECURE_NO_WARNINGS )
endif()

# 1. lua static library
if( BUILD_LIB)
    set(library_name lualib)
    set(source_files lapi.c lcode.c lctype.c
                    ldebug.c ldo.c ldump.c
                    lfunc.c lgc.c llex.c
                    lmem.c lobject.c lopcodes.c
                    lparser.c lstate.c lstring.c
                    ltable.c ltm.c lundump.c
                    lvm.c lzio.c lauxlib.c
                    lbaselib.c lbitlib.c lcorolib.c
                    ldblib.c liolib.c lmathlib.c
                    loslib.c lstrlib.c ltablib.c
                    loadlib.c linit.c)

    add_library (${library_name} STATIC ${source_files})
    set_target_properties(${library_name} PROPERTIES OUTPUT_NAME "lua") # rename the library output name

endif()

# 2. lua interpreter

if(BUILD_INTERPRETER AND BUILD_LIB)

    link_directories (${LUA_BINARY_DIR})
    add_executable (lua lua.c)
    target_link_libraries (lua ${library_name})

    if(UNIX)
            target_link_libraries( lua m )
    endif()

endif()

# 3. lua compiler
#link_directories (${LUA_BINARY_DIR})
#add_executable (luac luac.c)
#target_link_libraries (luac lualib)
#if(UNIX)
#        target_link_libraries( luac m )
#endif()
