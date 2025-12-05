cmake_minimum_required(VERSION 3.10.2)
project("quickjs-android")
add_subdirectory(./quickjs)
add_library(
        ${PROJECT_NAME}
        SHARED
        quickjs-jni.cpp
)

find_library(
        log-lib
        log)

# ----------------------------------------------------
# ✨ 关键修改：添加链接器标志以强制 16KB 页面对齐 ✨
# CMake 3.13之前版本不需要加 PRIVATE 标识，之后需要使用 PRIVATE 标识
# 16384
# ----------------------------------------------------
target_link_libraries(
        ${PROJECT_NAME}
        # ✨ 新增：使用 PRIVATE 关键词直接添加链接器标志 ✨
        PRIVATE
        "-Wl,--emit-relocs"
        "-Wl,-z,max-page-size=16384"
        ${log-lib}
        quickjs
)
