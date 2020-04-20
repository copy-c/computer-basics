# make 

make 

## camke 

```c
./Demo
    |
    +--- main.cc
    |
    +--- math/
          |
          +--- MathFunctions.cc
          |
          +--- MathFunctions.h

# CMake 最低版本号要求
cmake_minimum_required (VERSION 2.8)
# 项目信息
project (Demo1)
# 添加 math 子目录
add_subdirectory(math)
# 查找当前目录下的所有源文件
# 并将名称保存到 DIR_LIB_SRCS 变量
aux_source_directory(. DIR_LIB_SRCS)
# 生成链接库
add_library (MathFunctions ${DIR_LIB_SRCS}) // 将保存在DIR_LIB_SRCS变量中的文件都生成链接库
# 生成叫Demo的文件
add_executable(Demo main.cc)
# 添加链接库
target_link_libraries(Demo MathFunctions)
```

-L 和 -I 区别
-l 和 -i 区别
