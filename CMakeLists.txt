cmake_minimum_required(VERSION 3.10.2)
project(openal)

# Specify the C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# Set the ABI and min platform
set(ANDROID_ABI "armeabi-v7a x86_64 arm64-v8a")
set(ANDROID_PLATFORM android-19)

# Include directories
include_directories(
    ${CMAKE_SOURCE_DIR}/jni/OpenAL
    ${CMAKE_SOURCE_DIR}/jni/OpenAL/include
    ${CMAKE_SOURCE_DIR}/jni/OpenAL/OpenAL32/Include
)

# Source files
set(SOURCES
    jni/OpenAL/Alc/android.c
    jni/OpenAL/OpenAL32/alAuxEffectSlot.c
    jni/OpenAL/OpenAL32/alBuffer.c
    jni/OpenAL/Alc/audiotrack.c
    jni/OpenAL/OpenAL32/alAuxEffectSlot.c
    jni/OpenAL/OpenAL32/alBuffer.c
    jni/OpenAL/OpenAL32/alDatabuffer.c
    jni/OpenAL/OpenAL32/alEffect.c
    jni/OpenAL/OpenAL32/alError.c
    jni/OpenAL/OpenAL32/alExtension.c
    jni/OpenAL/OpenAL32/alFilter.c
    jni/OpenAL/OpenAL32/alListener.c
    jni/OpenAL/OpenAL32/alSource.c
    jni/OpenAL/OpenAL32/alState.c
    jni/OpenAL/OpenAL32/alThunk.c
    jni/OpenAL/Alc/ALc.c
    jni/OpenAL/Alc/alcConfig.c
    jni/OpenAL/Alc/alcEcho.c
    jni/OpenAL/Alc/alcModulator.c
    jni/OpenAL/Alc/alcReverb.c
    jni/OpenAL/Alc/alcRing.c
    jni/OpenAL/Alc/alcThread.c
    jni/OpenAL/Alc/ALu.c
    jni/OpenAL/Alc/bs2b.c
    jni/OpenAL/Alc/null.c
    jni/OpenAL/Alc/panning.c
    jni/OpenAL/Alc/mixer.c
)

# Define the library
add_library(openal SHARED ${SOURCES})

# Compiler options
target_compile_options(openal PRIVATE
    -DAL_ALEXT_PROTOTYPES
    -DANDROID
    -fpic
    -ffunction-sections
    -funwind-tables
    -fstack-protector
    -fno-short-enums
    -DHAVE_GCC_VISIBILITY
    -O3
    -g
)

# Link libraries
target_link_libraries(openal
    log
    android
    # Add other libraries as needed
)

# Conditional compilation flags
if(${ANDROID_ABI} STREQUAL "armeabi")
    target_compile_definitions(openal PRIVATE
        OPENAL_FIXED_POINT
        OPENAL_FIXED_POINT_SHIFT=16
    )
endif()
