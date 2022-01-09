---

layout: post
title: Switching generators
date: 2022-01-09 23:24:00
author: "phjung1"
header-img: "#"
tags:

- Cmake

---

# Switching generators

The code for this recipe is available at [cmake-cookbook/chapter-01/recipe-02 at v1.0 · dev-cafe/cmake-cookbook · GitHub](https://github.com/dev-cafe/cmake-cookbook/tree/v1.0/chapter-01/recipe-02) and has a C++, C, and Fortran example. The recipe is valid with CMake version 3.5 (and higher) and has been tested on GNU/Linux, macOS, and Windows.



CMake is a build system generator and a single CMakeLists.txt can be used to configure projects for different toolstacks on different platforms. You describe in CMakeLists.txt the operations the build system will have to run to get your code configured and compiled. Based on these instructions, CMake will generate the corresponding instructions for the chosen build system (Unix Makefiles, Ninja, Visual Studio, and so on). We will revisit generators in Chapter 13, *Alternative Generators and Cross-compilation*.



## Getting ready

CMake supports an extensive list of native build tools for different platforms. Both command-line tools, such as Unix Makefiles and Ninja, and integrated development environment (IDE) tools are supported. You can find an up-to-date list of the generators available on your platform and for your installed version of CMake by running the following:

    $ cmake --help


