include(FetchContent)
FetchContent_Populate(
  openxrsdk
  URL https://github.com/KhronosGroup/OpenXR-SDK/releases/download/release-1.0.25/OpenXR.Loader.1.0.25.nupkg
)
add_library(openxr_loader SHARED IMPORTED)

if(ANDROID)
  # require: ovr_openxr_mobile_sdk_xx.0.zip download from
  # https://developer.oculus.com/documentation/native/android/mobile-intro/
  set_target_properties(
    openxr_loader
    PROPERTIES
      IMPORTED_LOCATION
      ${CMAKE_CURRENT_LIST_DIR}/ovr_openxr_mobile_sdk/OpenXR/Libs/Android/${ANDROID_ABI}/Release/libopenxr_loader.so
      IMPORTED_GLOBAL true)
  set_property(
    TARGET openxr_loader
    PROPERTY INTERFACE_INCLUDE_DIRECTORIES ${openxrsdk_SOURCE_DIR}/include
             ${CMAKE_CURRENT_LIST_DIR}/ovr_openxr_mobile_sdk/OpenXR/include)
else()
  set_target_properties(
    openxr_loader
    PROPERTIES INTERFACE_INCLUDE_DIRECTORIES ${openxrsdk_SOURCE_DIR}/include
               IMPORTED_LOCATION
               ${openxrsdk_SOURCE_DIR}/native/x64/release/bin/openxr_loader.dll
               IMPORTED_IMPLIB
               ${openxrsdk_SOURCE_DIR}/native/x64/release/lib/openxr_loader.lib
               IMPORTED_GLOBAL true)
endif()
install(IMPORTED_RUNTIME_ARTIFACTS openxr_loader)
