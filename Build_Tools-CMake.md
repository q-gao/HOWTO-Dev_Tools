<!-- TOC -->

- [Quick Pick-up](#quick-pick-up)
    - [Generate `ninja` rules for different compiler](#generate-ninja-rules-for-different-compiler)
    - [Variables](#variables)
        - [cmake built-in variables](#cmake-built-in-variables)
            - [Check environments](#check-environments)
        - [User defined variables](#user-defined-variables)
    - [Command](#command)
        - [Advanced](#advanced)
- [Introduction](#introduction)
    - [Command Line](#command-line)
    - [Concepts](#concepts)
        - [Targets](#targets)
        - [Generators](#generators)
        - [Stages](#stages)
        - [Trees](#trees)
        - [CMake's global Variables](#cmakes-global-variables)
    - [CMake Commands](#cmake-commands)
        - [`message`](#message)
        - [`find_package`](#find_package)
        - [CMake's Command Line Arguments](#cmakes-command-line-arguments)

<!-- /TOC -->

[![alt](https://cmake.org/wp-content/uploads/2014/06/cmake_logo-main.png)](http://cmake.org)

# Quick Pick-up

[Quick intro on StackOverflow](https://stackoverflow.com/questions/17511496/how-to-create-a-shared-library-with-cmake)
> CRITICAL: generator has been detected ONLY after **`project`** statement
## CMake with Visual Studio
[Excellent doc on CMake, Visual Studio, and the Command Line](https://dmerej.info/blog/post/cmake-visual-studio-and-the-command-line/)
> Summary: use ninja to build with VS 

## Generate `ninja` rules for different compilers

Steps:
- First specify the compiler and linkers
- Generate ninja rules

```shell
set CMAKE=C:\Apps\Android\sdk\cmake\3.6.4111459\bin\cmake.exe
set  MAKE=C:\Apps\Android\sdk\cmake\3.6.4111459\bin\ninja.exe
set ANDROID_NDK_HOME=C:\Apps\Android\sdk\ndk-bundle
REM need to install cmake from Android SDK Manager
set CMAKE_TC=%ANDROID_NDK_HOME%\build\cmake\android.toolchain.cmake

%CMAKE% -DCMAKE_BUILD_TYPE=Release^
        -DCMAKE_TOOLCHAIN_FILE=%CMAKE_TC%^
        -DANDROID_ABI=arm64-v8a^
        -DCMAKE_MAKE_PROGRAM=%MAKE%^
        -G Ninja^
        ..
```

```batch
call "C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Auxiliary\Build\vcvarsall.bat" x64 

set  MAKE=C:\Apps\Android\sdk\cmake\3.6.4111459\bin\ninja.exe

cmake -DCMAKE_BUILD_TYPE=Release^
        -DCMAKE_MAKE_PROGRAM=%MAKE%^
        -G Ninja^
        ..
```

## Variables

### cmake built-in variables

```cmake
#
-DCMAKE_BUILD_TYPE=Release

# passed to compiler
cmake -DCMAKE_CXX_FLAGS="..."
cmake -DCMAKE_C_FLAGS="..."
```
#### Check environments
```cmake
if( NOT MSVC )
	include_directories("D:/code/fftw-3.3.8/api")
	link_directories("D:/code/fftw-3.3.8/build_android")

	add_executable(2dfft
	               simple_example.c
	)
	target_link_libraries(2dfft fftw3)
endif()
```

### User defined variables

```cmake
# set vs. option
set (CMAKE_BUILD_TYPE Release CACHE STRING "Build type")
option (BUILD_SHARED_LIBS "Build shared libraries" OFF)
```

```cmake
if(DEBUG_LEVEL_INFO_MEDIUM)
	add_definitions(-DDEBUG_LEVEL_INFO_MEDIUM)
endif(DEBUG_LEVEL_INFO_MEDIUM)
``` 

## Command

```cmake
# macro definition of preprocessor
add_definitions(-DDEBUG_LEVEL_INFO_MEDIUM)

# include and lib dir's
include_directories("D:/code/fftw-3.3.8/api")
link_directories("D:/code/fftw-3.3.8/build_android")

# specify exe dependency
add_executable(2dfft
               simple_example.c
)
target_link_libraries(2dfft fftw3)

# specify lib dependency
add_library(hdrplus
    EstMotion.cpp    
    MergeImg.cpp
)
```
### Advanced

```cmake
include(GNUInstallDirs)

include (CheckIncludeFile)
check_include_file (alloca.h         HAVE_ALLOCA_H)
```

# Introduction
CMake is a cross-platform build system generator. Projects specify their build process with platform-independent **CMake listfiles** included in each directory of a source tree with the name `CMakeLists.txt`

Good Tutorial
- Gitbook: Learning CMake: A beginner's guide https://www.gitbook.com/book/tuannguyen68/learning-cmake-a-beginner-s-guide/details
- 7 steps tutorial: https://cmake.org/cmake-tutorial/
- CMake Examples: https://cmake.org/Wiki/CMake/Examples
- CMake Variables: https://stackoverflow.com/questions/31037882/whats-the-cmake-syntax-to-set-and-use-variables

## Command Line

You should run the CMake command from within the binary dir, and pass the path to the directory containing the top-level `CMakeLists.txt`.

 CMake's command line parsing isn't great (e.g. see this [answer](https://stackoverflow.com/questions/14887438/spacing-in-d-option-in-cmake/17181954#17181954)). I'd recommend **avoiding leaving spaces after `-D` arguments**

Quick Examples
```sh
# opencv build 
cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D WITH_TBB=ON -D WITH_V4L=ON -D WITH_QT=ON -D WITH_OPENGL=ON ..
```

`-G` specifies the generator
```
# this will also print out the supported generator
cmake --help
```


## Concepts

### Targets

A CMake-based buildsystem is organized as a set of high-level logical targets. Each target corresponds to an executable or library, or is a custom target containing custom commands.

### Generators

`-G` to specify generator on command line

### Stages

- [**Stages**](http://cgold.readthedocs.io/en/latest/tutorials/cmake-stages.html):
  - [Configure step](http://cgold.readthedocs.io/en/latest/tutorials/cmake-stages.html#configure-step): parse top level `CMakeLists.txt` of source tree and create **CMakeCache.txt** file with cache variables. For CMake command-line this step is combined with generate step so terms configure and generate will be used interchangeably. The end of this step expressed by `Configuring done` message from CMake.
  - Generate step
  - Build Step

### Trees

Extracted from http://cgold.readthedocs.io/en/latest/tutorials.html
- **Directory Tree**
  - [Source Tree](http://cgold.readthedocs.io/en/latest/glossary/binary-tree.html#binary-tree):
  - [Binary Tree](http://cgold.readthedocs.io/en/latest/glossary/binary-tree.html#binary-tree): hierarchy of directories where CMake will store generated files and where native build tool will store it’s temporary files. Directory will contain variables/paths which are **specific to your environment** so they doesn’t mean to be shareable. Top level of the build tree is specified by the global variable
- **Build Type**
  - [Out-of-source Build](http://cgold.readthedocs.io/en/latest/tutorials/out-of-source.html#out-of-source): Keeping _binary tree_ in a separate directory from _source tree_ is a good practice and called out-of-source build.



### CMake's global Variables

| Global Variable    | Description                              |
| ------------------ | ---------------------------------------- |
| `CMAKE_SOURCE_DIR` | Top level dir of the **source tree**. CMake should be run from here. |
| `CMAKE_BINARY_DIR` | Top level dir of the **build tree**      |
| `CMAKE_GENERATOR`  | `set(CMAKE_GENERATOR "Ninja")`           |

## CMake Commands

### `message`

message("generator is set to ${CMAKE_GENERATOR}")

### `find_package`
CMake searches for package's `.cmake` file under installation **prefix**(i.e., directory). The set of installation **prefixes** is constructed using the following steps: see [here](https://cmake.org/cmake/help/v3.0/command/find_package.html)).

```shell
set(OpenCV_DIR "C:/opencv/build")  # <pkg>_DIR
find_package(OpenCV REQUIRED)
```



It has two modes to search for the named package:
- **Module** mode: search for a file `Find<package>.cmake` in `CMAKE_MODULE_PATH`
- **Config** mode: search for a file `<name>Config.cmake` or `<lower-case-name>-config.cmake`

### CMake's Command Line Arguments

```batch
cd mybuild
cmake.exe -G"Visual Studio 12 2013" -DOpenCV_DIR="C:/GithubPub/opencv/build" ..
```

## CMake Control

```cmake
    # see https://stackoverflow.com/questions/4346412/how-to-prepend-all-filenames-on-the-list-with-common-path
    # for function definitions
    set(CDM_FULLPATH_SRCS "")
    set(CDM_OBJS "")
    foreach(f ${CDM_SRCS_BASE})
        list( APPEND CDM_FULLPATH_SRCS ${CDM_DIR}/${f}.cpp)
        list( APPEND CDM_OBJS ${BINDIR}/${f}.o)
    endforeach(f)

set( PXL_CMP_LUA_OBJS "" )
foreach( f ${PXL_CMP_LUA_SRCS} )
    get_filename_component(b ${f} NAME_WE)
    list( APPEND PXL_CMP_LUA_OBJS  ${BINDIR}/${b}.o )
endforeach(f)
```
## CMake Functions

```cmake
FUNCTION(BASENAME results)
    # same as makefile's basename function
    set( listVar "" )
    FOREACH(f ${ARGN})
        get_filename_component(d ${f} DIRECTORY)
        get_filename_component(b ${f} NAME_WE)
        LIST(APPEND listVar "${d}/${b}")
    ENDFOREACH(f)    
    SET(${results} "${listVar}" PARENT_SCOPE)
ENDFUNCTION(BASENAME)

# BASENAME( bn a/b.cpp a2/b2.c)
# message(FATAL_ERROR  "BASENAME: ${bn}")

FUNCTION(FILENAMES_EXT_TO newList newExt)
    BASENAME(bnms ${ARGN})
    set( listVar "" )
    FOREACH(b ${bnms})
        LIST(APPEND listVar "${b}.${newExt}")
    ENDFOREACH(b)
    SET(${newList} "${listVar}" PARENT_SCOPE)
ENDFUNCTION(FILENAMES_EXT_TO)

FUNCTION(FILENAMES_RM_DIR_EXT_TO newList newExt)
    SET(listVar "")
    FOREACH(f ${ARGN})
        get_filename_component(b ${f} NAME_WE)
        LIST(APPEND listVar "${b}.${newExt}")
    ENDFOREACH(f)
    SET(${newList} "${listVar}" PARENT_SCOPE)
ENDFUNCTION(FILENAMES_RM_DIR_EXT_TO)

FUNCTION(PREPEND_DIR var prefix)
   SET(listVar "")
   FOREACH(f ${ARGN})
      LIST(APPEND listVar "${prefix}/${f}")
   ENDFOREACH(f)
   SET(${var} "${listVar}" PARENT_SCOPE)
ENDFUNCTION(PREPEND_DIR)

if(NOT DEFINED WORKDIR)
    if( ${CMAKE_SYSTEM_NAME} MATCHES "Linux" )
        execute_process( COMMAND getworkdir -v -fast OUTPUT_VARIABLE WORKDIR )
    else()
        set(WORKDIR $ENV{WORKDIR})
    endif()
endif()
```

