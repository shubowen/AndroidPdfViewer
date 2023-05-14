# For more information about using CMake with Android Studio, read the
# documentation: https://d.android.com/studio/projects/add-native-code.html

# Sets the minimum version of CMake required to build the native library.

cmake_minimum_required(VERSION 3.10.2)

# Declares and names the project.

project("jni_pdfium")

set(CMAKE_VERBOSE_MAKEFILE ON)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -O3")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O3")

set(src_files
        src/mainJNILib.cpp
        )

#set(CMAKE_CXX_STANDARD 17)


set(LIB_DIR ${CMAKE_SOURCE_DIR}/lib)

# libpdfium
add_library(libpdfium SHARED IMPORTED)
set_target_properties(libpdfium PROPERTIES IMPORTED_LOCATION ${LIB_DIR}/${ANDROID_ABI}/libpdfium.so)

# 源码
add_library(
        ${PROJECT_NAME}
        SHARED
        src/mainJNILib.cpp)

# 头文件
include_directories(
        include
        include/cpp
)

# liblog
find_library( # Defines the name of the path variable that stores the
        # location of the NDK library.
        log-lib
        # Specifies the name of the NDK library that
        # CMake needs to locate.
        log)

# libandroid
find_library( # Sets the name of the path variable.
        android-lib

        # Specifies the name of the NDK library that
        # you want CMake to locate.
        android )

# libjnigraphics
find_library( # Sets the name of the path variable.
        jnigraphics-lib

        # Specifies the name of the NDK library that
        # you want CMake to locate.
        jnigraphics)

target_compile_definitions(${PROJECT_NAME} PUBLIC -DHAVE_PTHREADS)

# link the necessary libraries
target_link_libraries(
        ${PROJECT_NAME}
#        libmodpng
#        libmodft2
#        libc++_shared
#        libmodpdfium
        libpdfium
        ${log-lib}
        ${android-lib}
        ${jnigraphics-lib}
        )