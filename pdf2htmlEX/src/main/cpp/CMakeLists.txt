# 16KB page-size support (Android 13+)
set(ANDROID_16K_PAGE_FLAGS "-Wl,-z,max-page-size=16384")

# Build envvar
add_library(envvar SHARED EnvVar.c)
set_target_properties(envvar PROPERTIES
    LINK_FLAGS "${ANDROID_16K_PAGE_FLAGS}"
)

# Build main pdf2htmlEX Android lib
add_library(pdf2htmlEX-android SHARED
    pdf2htmlEX.cc
    CCharGC.h
)

set_target_properties(pdf2htmlEX-android PROPERTIES
    LINK_FLAGS "${ANDROID_16K_PAGE_FLAGS}"
)

# Prevent huge debug symbols inside .so
target_compile_options(pdf2htmlEX-android PRIVATE -O3 -fvisibility=hidden)
target_compile_options(envvar PRIVATE -O3 -fvisibility=hidden)

# Link Android log lib
find_library(log-lib log)

# Link libraries
target_link_libraries(pdf2htmlEX-android
    pdf2htmlex::pdf2htmlex
    envvar
    ${log-lib}
)
