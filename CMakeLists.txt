cmake_minimum_required(VERSION 3.10)

project(lua)

set(LUASRC
    lapi.c
    lapi.h   
    lauxlib.c
    lauxlib.h
    lbaselib.c
    lcode.c
    lcode.h  
    lcorolib.c
    lctype.c
    lctype.h 
    ldblib.c
    ldebug.c
    ldebug.h 
    ldo.c
    ldo.h    
    ldump.c
    lfunc.c
    lfunc.h  
    lgc.c
    lgc.h    
    linit.c
    liolib.c
    llex.c
    llex.h   
    llimits.h
    lmathlib.c
    lmem.c
    lmem.h    
    loadlib.c
    lobject.c
    lobject.h 
    lopcodes.c
    lopcodes.h
    loslib.c
    lparser.c
    lparser.h 
    lprefix.h 
    lstate.c
    lstate.h  
    lstring.c
    lstring.h 
    lstrlib.c
    ltable.c
    ltable.h
    ltablib.c
    ltm.c
    ltm.h
    lua.h
    luaconf.h
    lualib.h
    lundump.c
    lundump.h
    lutf8lib.c
    lvm.c
    lvm.h
    lzio.c
    lzio.h
)

if(WIN32)
    add_library(liblua SHARED ${LUASRC})
    target_compile_definitions(liblua PRIVATE LUA_BUILD_AS_DLL)
else()
    add_library(liblua STATIC ${LUASRC})
    target_link_libraries(liblua PUBLIC readline)
    if (NOT APPLE)
        target_link_libraries(liblua PUBLIC m dl -Wl,-E)
        target_compile_definitions(liblua PUBLIC LUA_USE_LINUX)
    else()
        target_compile_definitions(liblua PUBLIC LUA_USE_MACOSX)
    endif()
endif()

set_target_properties(liblua PROPERTIES OUTPUT_NAME lua)
target_include_directories(liblua PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

add_executable(lua lua.c)
target_link_libraries(lua liblua)

if(WIN32)
    add_executable(luac ${LUASRC} luac.c)
else()
    add_executable(luac luac.c)
    target_link_libraries(luac liblua)
endif()