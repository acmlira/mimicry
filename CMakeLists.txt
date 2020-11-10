cmake_minimum_required(VERSION 3.1.0 FATAL_ERROR)
project(mimicry C CXX)

#
# Set some helper variables.
#
string(TOLOWER  "${CMAKE_SYSTEM_NAME}" targetSystem)
set(projectDir  "${CMAKE_CURRENT_LIST_DIR}")
set(sourceDir   "${projectDir}/sources")
set(targetName  "mimicry")

#
# Dependencies:
#   Using SDL and skia as a subproject for simplicity.
#   NOTE: This means that CMAKE_C_FLAGS set anywhere here are also passed to SDL and skia.
#
if(MINGW)
  set(VIDEO_OPENGLES OFF CACHE STRING "")
endif()

set(sdlDir "${projectDir}/externals/SDL")
add_subdirectory(${sdlDir} EXCLUDE_FROM_ALL)
set(SDL2MAIN_LIBRARY SDL2main)
set(SDL2_LIBRARY SDL2)

# Joysticks were causing trouble on android.
add_definitions(-DSDL_JOYSTICK=0)

if(ANDROID)
  set(skiaDir "${projectDir}/externals/skia/android/${ANDROID_ABI}")
else()
  set(skiaDir "${projectDir}/externals/skia/linux")
endif()
add_subdirectory("${skiaDir}" EXCLUDE_FROM_ALL)
set(SKIA_LIBRARY skia)

# SK_GL was causing trouble in skia CMake build 
add_definitions(-DSK_GL=1)

#
# Sources (relative to the project root dir).
#
set(projectSources
  sources/main.cpp
)

# Include dirs.
set(projectIncludeDirs ${projectIncludeDirs}
  "${sdlDir}/include"
  "${skiaDir}/../.."
)

message("\n-- Sources: ${projectSources}")
message("-- Include: ${projectIncludeDirs}\n")

#
# Platform dependent stuff.
#
if(MINGW)
  # -Link standard libs statically to reduce dll clutter.
  # -lmingw32 is needed to make WinMain not disappear when linking.
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -static -static-libgcc -lmingw32")

  # Only show the console in debug builds and also strip unused dependencies on MinSizeRel.
  set(CMAKE_EXE_LINKER_FLAGS_DEBUG "-mconsole")
  set(CMAKE_EXE_LINKER_FLAGS_RELEASE "-mwindows")
  set(CMAKE_EXE_LINKER_FLAGS_MINSIZEREL "-mwindows -s")
endif()

#
# Build the binary.
#
if(ANDROID)
  # On android the final binary is a shared library not an executable.
  add_library(${targetName} SHARED ${projectSources})
else()
  add_executable(${targetName} ${projectSources})
endif()

if(ANDROID)
  target_link_libraries(${targetName} ${SDL2MAIN_LIBRARY} ${SDL2_LIBRARY} ${SKIA_LIBRARY})
else()  
  target_link_libraries(${targetName} ${SDL2_LIBRARY} ${SKIA_LIBRARY})
endif()

target_include_directories(${targetName}
  PUBLIC ${projectIncludeDirs}
)


