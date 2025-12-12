cmake_minimum_required(VERSION 3.10)

# ================================
# IMPORTANT: Use static STL
# This avoids libc++_shared.so, which is 4KB pagesize and NOT compatible
# ================================
set(CMAKE_ANDROID_STL_TYPE c++_static)
set(CMAKE_STL_TYPE c++_static)

# Global 16KB pagesize (affects all shared libs)
set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -Wl,-z,max-page-size=16384")
set(CMAKE_MODULE_LINKER_FLAGS "${CMAKE_MODULE_LINKER_FLAGS} -Wl,-z,max-page-size=16384")
set(CMAKE_EXE_LINKER_FLAGS    "${CMAKE_EXE_LINKER_FLAGS}    -Wl,-z,max-page-size=16384")

# Function to apply 16KB on specific targets
function(enable_16kb target)
    target_link_options(${target} PRIVATE "-Wl,-z,max-page-size=16384")
endfunction()

# ===================================
# Libraries
# ===================================

add_library(envvar SHARED EnvVar.c)
enable_16kb(envvar)

add_library(pdf2htmlEX-android SHARED pdf2htmlEX.cc CCharGC.h)
enable_16kb(pdf2htmlEX-android)

# ===================================
# Android log
# ===================================
find_library(log-lib log)

# ===================================
# Linking
# ===================================
target_link_libraries(pdf2htmlEX-android
    pdf2htmlex::pdf2htmlex
    envvar
    ${log-lib}
)
