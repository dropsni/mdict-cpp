#设置工程名称
PROJECT(libmdict)
#自己定制
#SET(MY_TARGET demo)
#SET(MY_LINK test1 test2)


#设置CMAKE最小版本
CMAKE_MINIMUM_REQUIRED(VERSION 2.8)
#设置编译器
SET(CMAKE_CXX_COMPILER g++)
#设置构建类型，及相应的编译选项
SET(CMAKE_BUILD_TYPE "Debug")
SET(CMAKE_CXX_FLAGS_DEBUG "$ENV{CXXFLAGS} -O0 -Wall -g -ggdb --std=c++11 ")
SET(CMAKE_CXX_FLAGS_RELEASE "$ENV{CXXFLAGS} -O3 -Wall --std=c++11 ")

#设置执行文件输出目录
SET(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#设置库输出路径
SET(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

MESSAGE("++++++++++++++Start Build+++++++++++++++++")

#当前目录的文件
#添加子目录
ADD_SUBDIRECTORY(deps)

#头文件搜索目录
INCLUDE_DIRECTORIES(/usr/local/include .)
#链接库搜索路径
#LINK_DIRECTORIES(/usr/local/lib)
#设置链接库
#SET(EXTRA_LIBS ${EXTRA_LIBS} test1 test2)
#SET(EXTRA_LIBS ${EXTRA_LIBS} ${MY_LINK})



# mdict
#执行文件链接属性

find_library(HUNSPELL_LIBRARY
		NAMES hunspell-1.7
		HINTS "/usr/local/lib"
		NO_DEFAULT_PATH)
# check if we found the library
message(STATUS "HELLO_LIB: [${HELLO_LIB}]")

if (NOT HUNSPELL_LIBRARY)
	message(SEND_ERROR "Did not find lib hundspell")
endif()


ADD_EXECUTABLE(mydict mdict.cc binutils.cc xmlutils.cc ripemd128.c adler32.cc mdict_extern.cc mdict_extern_test.cc)
TARGET_LINK_LIBRARIES(mydict miniz ${HUNSPELL_LIBRARY})

ADD_LIBRARY(mdict mdict.cc binutils.cc xmlutils.cc ripemd128.c adler32.cc mdict_extern.cc)

# adler32 test
ADD_EXECUTABLE(adler32_test adler32.cc adler32_test.cc binutils.cc)

# ripemd128 test
ADD_EXECUTABLE(ripemd128_test ripemd128_test.c ripemd128.c)

# xmlutils
ADD_EXECUTABLE(xmlutils_test xmlutils.cc xmlutils_test.cc)

ADD_EXECUTABLE(binutils_test binutils.cc binutils_test.cc)
