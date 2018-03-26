project( lua  )

cmake_minimum_required( VERSION 3.7.0 )

set( CMAKE_CXX_STANDARD 14 )
set( CMAKE_C_STANDARD   11 )

set( LUA_PATH ${CMAKE_CURRENT_SOURCE_DIR} )
set( LUA_SRC_PATH ${CMAKE_CURRENT_SOURCE_DIR} )

#Корневой каталог сборки
set( LUA_INSTALL_TOP  "${CMAKE_CURRENT_BINARY_DIR}" )
#Каталог установки заголовочных файлов
set( LUA_INSTALL_INC  "${LUA_INSTALL_TOP}/include" )
#Каталог установки библиотечных файлов
set( LUA_INSTALL_LIB  "${LUA_INSTALL_TOP}/lib" )

#Список копируемых заголовочных файлов
set( LUA_TO_INC lua.h luaconf.h lualib.h lauxlib.h lua.hpp)

# Получение списка файлов сборки библиотеки lua
file( GLOB LUA_C_SOURCE ${LUA_SRC_PATH}/*.c )
file( GLOB LUA_H_SOURCE ${LUA_SRC_PATH}/*.h* )
set( LUA_SOURCE ${LUA_C_SOURCE} ${LUA_H_SOURCE} )

# Каталог инсталяции библиотеки и заг.файлов
set( CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${LUA_INSTALL_LIB} )

#Сборка библиотеки lua
add_library( ${PROJECT_NAME} STATIC ${LUA_SOURCE} )

#Копирование заголовочных файлов
foreach( lua_incl ${LUA_TO_INC} )

    if( EXISTS ${LUA_SRC_PATH}/${lua_incl})
        add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD
                           COMMAND ${CMAKE_COMMAND} -E copy_if_different
                           ${LUA_SRC_PATH}/${lua_incl}
                           ${LUA_INSTALL_INC}/${lua_incl} )
    endif()
endforeach()


#Установка переменных содержащих данные о сборке для родительского каталога
get_directory_property( HAS_PARENT PARENT_DIRECTORY )

if( HAS_PARENT )

  set( LUA_INCLUDES ${LUA_INSTALL_INC} PARENT_SCOPE )
  set( LUA_LIBS ${PROJECT_NAME} PARENT_SCOPE )

endif()


if( BUILD_BIN )
    set(interpret_name i${PROJECT_NAME})
    add_executable (${interpret_name} lua.c )
    target_link_libraries (${interpret_name} ${PROJECT_NAME})

    if(UNIX)
            target_link_libraries( lua m )
    endif()
endif()

#   Минимальный пример подключения lua как зависимости
#    project( lua  )
#    cmake_minimum_required( VERSION 3.7.0 )
#    set( CMAKE_CXX_STANDARD 14 )
#    set( CMAKE_C_STANDARD 11  )
#    set(LUA_ROOT ${CMAKE_SOURCE_DIR}/lua)
#    include_directories(${LUA_ROOT} )
#    include_directories( ${CMAKE_SOURCE_DIR}/src )
#    set (SOURCE_FILES src/main.cpp)
#    add_executable(luatest ${SOURCE_FILES})
#    add_subdirectory(${LUA_ROOT})
#    target_link_libraries(luatest ${LUA_LIBS} )
