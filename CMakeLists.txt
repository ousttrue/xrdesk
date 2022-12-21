cmake_minimum_required(VERSION 3.22.1)
project(xrdesk VERSION 0.1.0)

# set(CMAKE_CXX_STANDARD 14) set(CMAKE_POSITION_INDEPENDENT_CODE ON)

if(ANDROID)
  #
  # native_app_glue as a static lib
  #
  set(TARGET_NAME android_native_app_glue)
  set(ANDROID_NATIVE_APP_GLUE
      ${CMAKE_ANDROID_NDK}/sources/android/native_app_glue)
  add_library(${TARGET_NAME} STATIC
              ${ANDROID_NATIVE_APP_GLUE}/android_native_app_glue.c)
  target_include_directories(${TARGET_NAME} PUBLIC ${ANDROID_NATIVE_APP_GLUE})
  target_compile_options(${TARGET_NAME} PRIVATE -Wno-unused-parameter)

endif()

subdirs(thirdparty src/main/cpp)
