cmake_minimum_required(VERSION 3.20)

project(demo VERSION 1.0.0 LANGUAGES C)

# 强制 C 标准, 企业项目必须显式声明
set(CMAKE_C_STANDARD 17)
set(CMAKE_C_STANDARD_REQUIRED ON)
set(CMAKE_C_EXTENSIONS OFF)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

add_executable(${PROJECT_NAME}
    src/main.c
    src/math_utils.c
)

# 现代 CMake 核心: 所有配置挂在 Target 上, 绝不使用全局 include_directories
target_include_directories(${PROJECT_NAME} PRIVATE
    ${CMAKE_SOURCE_DIR}/include
)

# 编译告警: PRIVATE 限定仅本目标生效, 不污染其他依赖
target_compile_options(${PROJECT_NAME} PRIVATE
    $<$<C_COMPILER_ID:GNU,Clang>:-Wall -Wextra -Wpedantic -Werror>
)

# [关键] 可执行文件输出到 build/bin
set_target_properties(${PROJECT_NAME} PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin
)
