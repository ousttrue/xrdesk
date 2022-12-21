include(FetchContent)
FetchContent_Populate(
  openxrsdk
  URL https://github.com/KhronosGroup/OpenXR-SDK/releases/download/release-1.0.25/OpenXR.Loader.1.0.25.nupkg
)
add_library(openxr_loader SHARED IMPORTED)
set_target_properties(
  openxr_loader
  PROPERTIES INTERFACE_INCLUDE_DIRECTORIES ${openxrsdk_SOURCE_DIR}/include
             IMPORTED_LOCATION
             ${openxrsdk_SOURCE_DIR}/native/x64/release/bin/openxr_loader.dll
             IMPORTED_IMPLIB
             ${openxrsdk_SOURCE_DIR}/native/x64/release/lib/openxr_loader.lib
             IMPORTED_GLOBAL true)
install(IMPORTED_RUNTIME_ARTIFACTS openxr_loader)
