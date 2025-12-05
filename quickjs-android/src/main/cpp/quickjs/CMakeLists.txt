cmake_minimum_required(VERSION 3.10.2)
project(quickjs CXX)
include_directories(quickjs)
set(SOURCE_DIR
        quickjs/cutils.c
        quickjs/libbf.c
        quickjs/libregexp.c
        quickjs/libunicode.c
        quickjs/quickjs.c
        )

file(STRINGS "quickjs/VERSION" CONFIG_VERSION)
add_definitions(-DCONFIG_VERSION="${CONFIG_VERSION}")
add_definitions(-DCONFIG_BIGNUM)
add_definitions(-D_GNU_SOURCE)
add_definitions(-DCONFIG_CC="gcc")
add_definitions(-DCONFIG_PREFIX="/usr/local")
add_library(
        ${PROJECT_NAME}
        SHARED
        ${SOURCE_DIR}
)

# ----------------------------------------------------
# ✨ 关键修改：添加链接器标志以强制 16KB 页面对齐 ✨
# CMake 3.13之前版本不需要加 PRIVATE 标识，之后需要使用 PRIVATE 标识
# 16384
# ----------------------------------------------------
target_link_libraries(
        ${PROJECT_NAME}
        PUBLIC
        "-Wl,--emit-relocs"
        "-Wl,-z,max-page-size=16384"
)
target_include_directories(${PROJECT_NAME} PUBLIC .)


