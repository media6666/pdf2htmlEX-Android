cmake_minimum_required(VERSION 3.14 FATAL_ERROR)

project(pdf2htmlEX-android)

# Bắt buộc với Conan toolchain + Android NDK 26
set(CMAKE_BUILD_TYPE Release CACHE STRING "Build configuration")

# Chuẩn STL (NDK 26)
set(CMAKE_ANDROID_STL_TYPE c++_shared)

# Để các thư viên con cũng nhận flag này
set(CMAKE_POSITION_INDEPENDENT_CODE ON)

# Tìm package pdf2htmlEX (Conan prefab)
find_package(pdf2htmlEX REQUIRED CONFIG)

# Build phần native
add_subdirectory(src/main/cpp)
