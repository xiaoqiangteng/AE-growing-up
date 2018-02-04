# Colmap

## Cmake

1. include(CheckCXXCompilerFlag)

检查C++编译器版本

例子：

    include(CheckCXXCompilerFlag)
    CHECK_CXX_COMPILER_FLAG("-std=c++11" COMPILER_SUPPORTS_CXX11)
    CHECK_CXX_COMPILER_FLAG("-std=c++0x" COMPILER_SUPPORTS_CXX0X)
    if(COMPILER_SUPPORTS_CXX11)
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
    elseif(COMPILER_SUPPORTS_CXX0X)
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
    else()
        message(STATUS "The compiler ${CMAKE_CXX_COMPILER} has no C++11 support. Please use a different C++ compiler.")
    endif()
   
2. cmake的常用变量
 
CMAKE\_BINARY\_DIR,PROJECT\_BINARY\_DIR,\_BINARY\_DIR：
这三个变量内容一致，如果是内部编译，就指的是工程的顶级目录，如果是外部编译，指的就是工程编译发生的目录。

CMAKE\_SOURCE\_DIR,PROJECT\_SOURCE\_DIR,\_SOURCE\_DIR：
这三个变量内容一致，都指的是工程的顶级目录。

CMAKE\_CURRENT\_BINARY\_DIR：外部编译时，指的是target目录，内部编译时，指的是顶级目录

CMAKE\_CURRENT\_SOURCE\_DIR：CMakeList.txt所在的目录

CMAKE\_CURRENT\_LIST\_DIR：CMakeList.txt的完整路径

CMAKE\_CURRENT\_LIST\_LINE：当前所在的行

CMAKE\_MODULE\_PATH：如果工程复杂，可能需要编写一些cmake模块，这里通过SET指定这个变量

LIBRARY\_OUTPUT\_DIR,BINARY\_OUTPUT\_DIR：库和可执行的最终存放目录

find_package可以被用来在系统中自动查找配置构建工程所需的程序库