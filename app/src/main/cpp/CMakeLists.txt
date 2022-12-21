#
# xrdesk
#
set(TARGET_NAME xrdesk)

if(ANDROID)
  add_library(${TARGET_NAME} SHARED platformplugin_android.cpp
                                    graphicsplugin_opengles.cpp)
  add_compile_definitions(${TARGET_NAME} PRIVATE XR_USE_PLATFORM_ANDROID
                          XR_USE_GRAPHICS_API_OPENGLES)
  target_link_libraries(
    ${TARGET_NAME}
    android
    log
    android_native_app_glue
    openxr_loader
    GLESv3
    EGL)
  target_link_options(${TARGET_NAME} PUBLIC -u ANativeActivity_onCreate)
else()
  add_executable(${TARGET_NAME} platformplugin_win32.cpp
                                graphicsplugin_opengl.cpp)
  add_compile_definitions(${TARGET_NAME} PRIVATE XR_USE_PLATFORM_WIN32
                          XR_USE_GRAPHICS_API_OPENGL)
  target_link_libraries(${TARGET_NAME} openxr_loader OpenGL32.lib)

  install(
    FILES $<TARGET_PDB_FILE:${TARGET_NAME}>
    DESTINATION bin
    OPTIONAL)

endif()

target_sources(
  ${TARGET_NAME}
  PRIVATE main.cpp openxr_program.cpp logger.cpp platformplugin_factory.cpp
          graphicsplugin_factory.cpp common/gfxwrapper_opengl.c)
target_include_directories(
  ${TARGET_NAME} PRIVATE ${CMAKE_CURRENT_LIST_DIR}/include
                         ${CMAKE_CURRENT_LIST_DIR})
if(GLSLANG_VALIDATOR AND NOT GLSLC_COMMAND)
  target_compile_definitions(${TARGET_NAME} PRIVATE USE_GLSLANGVALIDATOR)
endif()
install(TARGETS xrdesk)
