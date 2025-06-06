#1. 确定CMake的最低版本需求
CMAKE_MINIMUM_REQUIRED(VERSION 3.0.0)

#2. 确定工程名
PROJECT(WtBtPorter LANGUAGES CXX)
SET(CMAKE_CXX_STANDARD 17)

SET(LIBRARY_OUTPUT_PATH ${CMAKE_BINARY_DIR}/build_${PLATFORM}/${CMAKE_BUILD_TYPE}/bin/WtBtPorter)

#7. 添加源码
file(GLOB SRCS *.cpp)
file(GLOB HDRS *.h)

IF(MSVC)
	list (APPEND SRCS ../Common/mdump.cpp)
ENDIF()

INCLUDE_DIRECTORIES(${INCS})
LINK_DIRECTORIES(${LNKS})
ADD_LIBRARY(WtBtPorter SHARED ${SRCS} ${HDRS})

IF(MSVC)
	SET(LIBS
		WtBtCore
		WTSTools
		WTSUtils
	)
ELSE(GNUCC)
	SET(LIBS
		WtBtCore
		WTSTools
		WTSUtils

		dl
		pthread
		boost_filesystem
	)
	if(WIN32)
		LIST(APPEND LIBS iconv)
	ENDIF()
ENDIF()


TARGET_LINK_LIBRARIES(WtBtPorter ${LIBS})
IF (WIN32)
ELSE (UNIX)
	SET_TARGET_PROPERTIES(WtBtPorter PROPERTIES
		CXX_VISIBILITY_PRESET hidden
		VISIBILITY_INLINES_HIDDEN 1
        LINK_FLAGS_RELEASE -s)
ENDIF ()

