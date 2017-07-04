# CMake
Quick Examples
```sh
# opencv
cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D WITH_TBB=ON -D WITH_V4L=ON -D WITH_QT=ON -D WITH_OPENGL=ON ..
```

[![alt](https://cmake.org/wp-content/uploads/2014/06/cmake_logo-main.png)](http://cmake.org)
CMake is a cross-platform build system generator. Projects specify their build process with platform-independent **CMake listfiles** included in each directory of a source tree with the name `CMakeLists.txt`

Good Tutorial
- Gitbook: Learning CMake: A beginner's guide https://www.gitbook.com/book/tuannguyen68/learning-cmake-a-beginner-s-guide/details
- 7 steps tutorial: https://cmake.org/cmake-tutorial/
- CMake Examples: https://cmake.org/Wiki/CMake/Examples
- CMake Variables: https://stackoverflow.com/questions/31037882/whats-the-cmake-syntax-to-set-and-use-variables

## Concepts

### Targets

A CMake-based buildsystem is organized as a set of high-level logical targets. Each target corresponds to an executable or library, or is a custom target containing custom commands.

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
Global Variable  |  Description
-----------------|----------
`CMAKE_SOURCE_DIR` | Top level dir of the **source tree**. CMake should be run from here.
`CMAKE_BINARY_DIR` | Top level dir of the **build tree**

## CMake Commands

### `find_package`
CMake searches for package's `.cmake` file under installation **prefix**(i.e., directory. The set of installation **prefixes** is constructed using the following steps: see [here](The set of installation prefixes is constructed using the following steps.)).

It has two modes to search for the named package:
- **Module** mode: search for a file `Find<package>.cmake` in `CMAKE_MODULE_PATH`
- **Config** mode: search for a file `<name>Config.cmake` or `<lower-case-name>-config.cmake`

### CMake's Command Line Arguments
