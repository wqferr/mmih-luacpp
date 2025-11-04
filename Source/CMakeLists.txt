cmake_minimum_required(VERSION 3.30.0)
project(LuaCpp)

set(CMAKE_CXX_STANDARD 17)

# Source files
set(SOURCE_FILES
    Lua.hpp
    LuaCpp.hpp
    LuaVersion.cpp LuaVersion.hpp
    Engine/LuaState.cpp Engine/LuaState.hpp
    Engine/LuaType.cpp Engine/LuaType.hpp
    Engine/LuaTNil.cpp Engine/LuaTNil.hpp
    Engine/LuaTString.cpp Engine/LuaTString.hpp
    Engine/LuaTNumber.cpp Engine/LuaTNumber.hpp
    Engine/LuaTBoolean.cpp Engine/LuaTBoolean.hpp
    Engine/LuaTTable.cpp Engine/LuaTTable.hpp
    Engine/LuaTUserData.cpp Engine/LuaTUserData.hpp
    Registry/LuaRegistry.cpp Registry/LuaRegistry.hpp
    Registry/LuaCodeSnippet.cpp Registry/LuaCodeSnippet.hpp
    Registry/LuaCompiler.cpp Registry/LuaCompiler.hpp
    Registry/LuaCFunction.cpp Registry/LuaCFunction.hpp
    Registry/LuaLibrary.cpp Registry/LuaLibrary.hpp
    LuaContext.cpp LuaContext.hpp
    LuaMetaObject.cpp LuaMetaObject.hpp
)

include(GNUInstallDirs)

add_library(luacpp SHARED ${SOURCE_FILES})
add_library(luacpp_static STATIC ${SOURCE_FILES})
set_target_properties(luacpp_static PROPERTIES OUTPUT_NAME luacpp)

target_link_libraries(luacpp PRIVATE lua_static)
target_link_libraries(luacpp_static PRIVATE lua_static)

# #######
# Install
# #######
include(CMakePackageConfigHelpers)

set(CMAKE_CONFIG_DEST "${CMAKE_INSTALL_LIBDIR}/${PROJECT_NAME}/cmake")
set(LuaCpp_INCLUDE_DIR "${CMAKE_INSTALL_INCLUDEDIR}/${PROJECT_NAME}")
set(LuaCpp_INSTALL_LIBDIR "${CMAKE_INSTALL_LIBDIR}")

install(TARGETS luacpp
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR})

install(TARGETS luacpp_static
    DESTINATION ${CMAKE_INSTALL_LIBDIR})

install(FILES LuaCpp.hpp Lua.hpp LuaContext.hpp LuaMetaObject.hpp LuaVersion.hpp
    DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}/${PROJECT_NAME}")

install(DIRECTORY ${CMAKE_SOURCE_DIR}/Registry ${CMAKE_SOURCE_DIR}/Engine
    DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}/${PROJECT_NAME}"
    FILES_MATCHING
    PATTERN "*.hpp"
)
configure_package_config_file(
    LuaCppConfig.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/LuaCppConfig.cmake
    INSTALL_DESTINATION "${CMAKE_CONFIG_DEST}"
    PATH_VARS
    LuaCpp_INCLUDE_DIR
    LuaCpp_INSTALL_LIBDIR
)
write_basic_package_version_file(
    ${CMAKE_CURRENT_BINARY_DIR}/LuaCppConfigVersion.cmake
    VERSION 0.1.0
    COMPATIBILITY SameMajorVersion
    ARCH_INDEPENDENT
)
install(FILES
    ${CMAKE_CURRENT_BINARY_DIR}/LuaCppConfigVersion.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/LuaCppConfig.cmake
    DESTINATION ${CMAKE_CONFIG_DEST}
)
