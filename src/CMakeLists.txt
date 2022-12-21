#
# xrdesk
#
set(TARGET_NAME xrdesk)
add_executable(
  ${TARGET_NAME}
  main.cpp
  openxr_program.cpp
  logger.cpp
  platformplugin_factory.cpp
  platformplugin_win32.cpp
  graphicsplugin_factory.cpp
  graphicsplugin_opengl.cpp
  common/gfxwrapper_opengl.c)
add_compile_definitions(${TARGET_NAME} PRIVATE XR_USE_PLATFORM_WIN32
                        XR_USE_GRAPHICS_API_OPENGL)
target_link_libraries(${TARGET_NAME} openxr_loader OpenGL32.lib)

target_include_directories(
  ${TARGET_NAME} PRIVATE ${CMAKE_CURRENT_LIST_DIR}/include
                         ${CMAKE_CURRENT_LIST_DIR})

if(GLSLANG_VALIDATOR AND NOT GLSLC_COMMAND)
  target_compile_definitions(${TARGET_NAME} PRIVATE USE_GLSLANGVALIDATOR)
endif()

install(TARGETS xrdesk)
install(
  FILES $<TARGET_PDB_FILE:${TARGET_NAME}>
  DESTINATION bin
  OPTIONAL)
