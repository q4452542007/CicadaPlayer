cmake_minimum_required(VERSION 3.6)
project(dataSourceTest LANGUAGES CXX)

# require C++11
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

if (APPLE)
        include(../Apple.cmake)
endif ()

cmake_policy(SET CMP0079 NEW)
include(../GoogleTest.cmake)
add_executable(dataSourceTest "")

target_sources(dataSourceTest
        PRIVATE
        dataSourceUnitTest.cpp
        )

target_include_directories(
        dataSourceTest
        PRIVATE
        ../../../plugin
        )

target_link_libraries(
        dataSourceTest
        demuxer
        videodec
        data_source
 #       plugin
        framework_utils
        avformat
        avcodec
        swresample
        avutil
        swscale
        z
        curl
        ssl
        crypto
        gtest_main)
if (APPLE)
        target_link_libraries(
                dataSourceTest
                iconv
                bz2
                ${FRAMEWORK_LIBS}
        )
else()
        target_link_libraries(
                dataSourceTest
                dl
                pthread
        )

endif ()
enable_testing()

add_test(
        NAME google_test
        COMMAND $<TARGET_FILE:dataSourceTest>
)