cmake_minimum_required(VERSION 3.15)

if(NOT DEFINED CMAKE_TOOLCHAIN_FILE)
    if(DEFINED ENV{VITASDK})
        set(CMAKE_TOOLCHAIN_FILE "$ENV{VITASDK}/share/vita.toolchain.cmake" CACHE PATH "toolchain file")
    else()
        message(FATAL_ERROR "Please define VITASDK to point to your SDK path!")
    endif()
endif()

project(sokol_sample)
include("${VITASDK}/share/vita.cmake" REQUIRED)


## Configuration options for this app
# Display name (under bubble in LiveArea)
set(APP_NAME "Sokol Sample")
# Unique ID must be exactly 9 characters. Recommended: XXXXYYYYY where X =
# unique string of developer and Y = a unique number for this app
set(APP_ID  "SKGL20002")
# Optional version string to show in LiveArea's more info screen
set(APP_VERSION  "01.00")

## Flags and includes for building
# Note that we make sure not to overwrite previous flags
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=gnu11 -Wall")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
# Optional. You can specify more param.sfo flags this way.

# Add any additional include paths here
include_directories(PVR_PSP2)

# Add any additional library paths here
# ${CMAKE_CURRENT_BINARY_DIR} lets you use any library currently being built
link_directories(PVR_PSP2)

## Build and link
# Add all the files needed to compile here
add_executable(${PROJECT_NAME}
  src/main.c
)

# Library to link to (drop the -l prefix). This will mostly be stubs.
target_link_libraries(${PROJECT_NAME}
  SceCtrl_stub
  SceDisplay_stub
  SceTouch_stub
  libIMGEGL_stub_weak
  libgpu_es4_ext_stub_weak
  libGLESv2_stub_weak
  m
)

## Create Vita files
vita_create_self(${PROJECT_NAME}.self ${PROJECT_NAME} UNSAFE)
# The FILE directive lets you add additional files to the VPK, the syntax is
# FILE src_path dst_path_in_vpk. In this case, we add the LiveArea paths.
vita_create_vpk(${PROJECT_NAME}.vpk ${APP_ID} ${PROJECT_NAME}.self
  VERSION ${APP_VERSION}
  NAME ${APP_NAME}

  FILE sce_sys/icon0.png sce_sys/icon0.png
  FILE sce_sys/livearea/contents/bg.png sce_sys/livearea/contents/bg.png
  FILE sce_sys/livearea/contents/startup.png sce_sys/livearea/contents/startup.png
  FILE sce_sys/livearea/contents/template.xml sce_sys/livearea/contents/template.xml

  FILE module/libGLESv2.suprx module/libGLESv2.suprx
  FILE module/libgpu_es4_ext.suprx module/libgpu_es4_ext.suprx
  FILE module/libIMGEGL.suprx module/libIMGEGL.suprx
  FILE module/libpvrPSP2_WSEGL.suprx module/libpvrPSP2_WSEGL.suprx
)
