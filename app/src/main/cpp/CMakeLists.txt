find_package(Vulkan REQUIRED)

#
# xrdesk
#
set(TARGET_NAME xrdesk)

if(ANDROID)
  #
  # native_app_glue as a static lib
  #
  # set(TARGET_NAME android_native_app_glue)
  set(ANDROID_NATIVE_APP_GLUE
      ${CMAKE_ANDROID_NDK}/sources/android/native_app_glue)
  add_library(android_native_app_glue STATIC
              ${ANDROID_NATIVE_APP_GLUE}/android_native_app_glue.c)
  target_include_directories(android_native_app_glue
                             PUBLIC ${ANDROID_NATIVE_APP_GLUE})
  target_compile_options(android_native_app_glue PRIVATE -Wno-unused-parameter)

  add_library(${TARGET_NAME} SHARED platformplugin_android.cpp
                                    graphicsplugin_vulkan.cpp)
  add_compile_definitions(${TARGET_NAME} PRIVATE XR_USE_PLATFORM_ANDROID
                          XR_USE_GRAPHICS_API_VULKAN)
  target_link_libraries(${TARGET_NAME} android log android_native_app_glue
                        openxr_loader Vulkan::Vulkan)
  target_link_options(${TARGET_NAME} PUBLIC -u ANativeActivity_onCreate)
else()
  add_executable(${TARGET_NAME} platformplugin_win32.cpp
                                graphicsplugin_vulkan.cpp)
  add_compile_definitions(${TARGET_NAME} PRIVATE XR_USE_PLATFORM_WIN32
                          XR_USE_GRAPHICS_API_VULKAN)
  target_link_libraries(${TARGET_NAME} openxr_loader Vulkan::Vulkan)

  install(
    FILES $<TARGET_PDB_FILE:${TARGET_NAME}>
    DESTINATION bin
    OPTIONAL)

endif()

target_sources(
  ${TARGET_NAME} PRIVATE main.cpp openxr_program.cpp logger.cpp
                         platformplugin_factory.cpp graphicsplugin_factory.cpp)
target_include_directories(
  ${TARGET_NAME}
  PRIVATE ${CMAKE_CURRENT_LIST_DIR}/include ${CMAKE_CURRENT_LIST_DIR}
          ${CMAKE_CURRENT_LIST_DIR}/vulkan_shaders)
if(GLSLANG_VALIDATOR AND NOT GLSLC_COMMAND)
  target_compile_definitions(${TARGET_NAME} PRIVATE USE_GLSLANGVALIDATOR)
endif()
install(TARGETS xrdesk)
