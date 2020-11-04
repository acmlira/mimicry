<div align="center">

# Mimicry

</div>

_A simple crossplatform SDL2 + skia CMake build environment example/test. Based on [simpleSDL](https://github.com/suikki/simpleSDL) (thank you so much!)._

Tested to build and run successfully with:
  - Linux (Ubuntu): Clang
  - Android: Gradle + NDK
  - Windows: mingw-w64 or Visual Studio 2015

Table of contents:
1. [Getting started](#getting-started)
    1. [Requirements](#requirements)
    2. [Clone](#clone)
    3. [Preparing skia](#preparing-skia)
2. [Compilling](#compilling)
    1. [Linux](#linux)
    2. [Android](#android)
    3. [Windows](#windows)
3. [Contributing](#contributing)

## Getting started

<div align="center">

_WIP_

</div>

### Clone

<div align="center">

_WIP_

</div>

### Requirements

<div align="center">

_WIP_

</div>

### Preparing skia

The standard skia build does not have CMake compatibility so we will transform the build. [Learn more about the skia build](https://skia.org/user/build). 

Step-by-step:

1. Run `python2 tools/git-sync-deps` inside `externals`;

2. Generate CMake project:

   ```
   bin/gn gen out/cmake --ide=json --json-ide-script=../../gn/gn_to_cmake.py --args='is_official_build=true'
   ```

   to build skia as a static library or

   ```
   bin/gn gen out/cmake --ide=json --json-ide-script=../../gn/gn_to_cmake.py --args='is_official_build=true is_component_build=true'
   ```

   to build skia as a dynamic library. To see all available arguments:

   ```
   bin/gn args out/cmake --list
   ```
   
   suggestion for Android build:
   ```
   bin/gn gen out/cmake --ide=json --json-ide-script=../../gn/gn_to_cmake.py --args='
            skia_use_system_libjpeg_turbo = false
            skia_use_system_libwebp = false
            skia_use_system_expat = false
            skia_use_system_libpng = false
            skia_use_system_freetype2 = false
            ndk="/home/you/Android/Sdk/ndk/21.3.6528147"
            target_os="android" 
            target_cpu="arm"
            is_debug=false
            is_component_build=true
            extra_cflags=["-O3"]
            skia_use_angle = false
            skia_use_icu = false
            skia_use_lua = false
            skia_use_opencl = false
            skia_use_piex = false
            skia_use_zlib = false
            skia_enable_tools = false
            skia_enable_pdf = false
            skia_use_gl = true
            skia_enable_skottie = false
            '
   ```

## Compilling

<div align="center">

_WIP_

</div>

### Linux

Just run `build.sh` inside `platforms` folder.

   <div align="center">

   _or_

   </div>

Alternatively you can do something like this in the project root dir:

```bash
mkdir build/make_debug
cd build/make_debug
cmake -DCMAKE_BUILD_TYPE:STRING=Debug ../..
make
```

### Android

_Make sure you have NDK and CMake plugins installed on Android SDK
(https://developer.android.com/studio/projects/add-native-code.html)._

Step-by-step:

1. Make sure you have installed:
   1. Android SDK Build Tools 30;
   2. NDK 21;
   3. Android CMake 10.
  
2. Have the `ANDROID_HOME` environment variable;

3. Copy all content from 
   ```
   externals/SDL/android-project/app/src/main/java/org/libsdl/app
   ```
   and put it into
   ```
   platforms/android/android-app/src/main/java/org/libsdl/app/
   ```

4. Pay attention to the skia build and see if all the arguments are in the `arg.gn` file. See build options [here](https://skia.org/user/build#android).

5. Run `gradlew assemble` in `platforms/android`

   <div align="center">

   _or_

   </div>

   Open the project in Android Studio and build using the IDE. NOTE: Make sure
   to open the `platforms/android/` dir. Android studio can also
   open the root dir but it's not recognized as an android project.

The included Android Gradle CMake project is pretty much what Android Studio
generates when you create a new empty app with native CMake support. Just
pointing to the CMakeLists.txt in the project root.

### Windows

<div align="center">

_WIP_

</div>

## Contributing

<div align="center">

_WIP_

</div>