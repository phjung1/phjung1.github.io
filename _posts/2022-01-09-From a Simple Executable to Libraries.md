---
layout: post
title: From a Simple Executable to Libraries
date: 2022-01-09 22:26:00
author: "phjung1"
header-img: "#"
tags:

- Cmake


---

# From a Simple Executable to Libraries

In this chapter, we will cover the following recipes:

- Compiling a single source file into an executable

- Switching generators

- Building and linking static and shared libraries

- Controlling compilation with conditionals

- Presenting options to the user

- Specifying the compuler

- Switching the build type

- Controlling compuer flags

- Settings the standard for the language

- Using control flow constructs

## Introduction

The recipes in this chapter will walk you through fairly basic tasks needed to build your code: compiling an executable, compiling a library, performing build actions based on user input, and so forth. CMake is a *build system generator* particularly suited to being platform- and compiler-independent. We have striven to show this aspect in this chapter. Unless stated otherwise, all recipes are independent of the operating system; they can be run without modifications on GNU/Linux, macOS, and Windows.

The recipes in this book are mainly designed for C++ projects and demonstrated using C++ examples, but CMake can be used for projects in other languages, including C and Fortran. For any given recipe and whenever it makes sense, we have tried to include examples in C++, C, and Fortran. In this way, you will be able to choose the recipe in your favorite flavor. Some recipes are tailor-made to highlight challenges to overcome when a specific language is chosen.

## Compiling a single source file into an executable

The code for this recipe is available at [cmake-cookbook/chapter-01/recipe-01 at v1.0 · dev-cafe/cmake-cookbook · GitHub](https://github.com/dev-cafe/cmake-cookbook/tree/v1.0/chapter-01/recipe-01) and has C++, C, and Fortran examples. The recipe is valid with CMake version 3.5 (and higher) and has been tested on GNU/Linux, macOS, and Windows.

In this recipe, we will demonstrate how to run CMake to configure and build a simple project. The project consists of a single source file for a single executable. We will discuss the project in C++, but examples for C and Fortran are available in the GitHub repository.

### Getting ready

We with to compile the following source code into a single executable:

```cpp
#include <cstdlib>
#include <iostream>
#include <string>

std::string say_hello() { return std::string('Hello, CMake world!'); }

int main() {
  std::cout << say_hello() << std::endl;
  return EXIT_SUCCESS;
}
```

### how to do it

Alongside the source file, we need to provide CMake with a description of the operations to perform to configure the project for the build tools. The description is done in the CMake language, whose comprehensive documentation can be found online at [CMake Reference Documentation &mdash; CMake 3.22.1 Documentation](https://cmake.org/cmake/help/latest/). We will place the CMake instructions into a file called CMakeLists.txt.

    The name of the file is case sensitive; it has to be called CMakeLists.txt for CMake to be able to parse it.

In detail, these are the steps to follow:

1. Open a text file with your favorite editor. The name of this file will be CMakeLists.txt.

2. The first line sets a minimum required version for CMake. A fatal error will be issued if a version of CMake lower than that is used:
   
    cmake_minimum_required(VERSION 3.5 FATAL_ERROR)

3. The second line declares the name of the project (recipe-01) and the supported language (CXX stands for C++):
   
    project(recipe-01 LANGUAGES CXX)

4. We instruct CMake to create a new *target*: the executable hello-world. This executable is generated by compiling and linking the source file hello-world.cpp. CMake will use default settings for the compiler and build automation tools selected:
   
    add_executable(hello-world hello-world.cpp)

5. Save the file in the same directory as the source file hello-world.cpp. Remember that it can only be named CMakeLists.txt.

6. We are now ready to configure the project by creating and stepping into a build directory:
   
    $ mkdir -p build
    $ cd build
    $ cmake ..
   
    -- The CXX compiler identification is GNU 8.1.0
    -- Check for working CXX compiler: /usr/bin/c++
    -- Check for working CXX compiler: /usr/bin/c++ -- works
    -- Detecting CXX compiler ABI info
    -- Detecting CXX compiler ABI info - done
    -- Detecting CXX compile features
    -- Detecting CXX compile features - done
    -- Configuring done
    -- Generating done
    -- Build files have been written to: /home/user/cmake-cookbook/chapter-01/recipe-01/cxx-example/build

7. if everthing went well, the configuration for the proejct has been generated in the build directory. We can now compile the executable:
   
    $ cmake --build .
   
    Scanning dependencies of target hello-world
    [ 50%] Building CXX object CMakeFiles/hello-world.dir/hello-world.cpp.o
    [100%] Linking CXX executable hello-world
    [100%] Built target hello-world

### how it works

In this recipte, we have used a simple CMakeLists.txt to build a 'hello world' executable:

    cmake_minimum_required(VERSION 3.5 FATAL_ERROR)
    
    project(recipe-01 LANGUAGES CXX)
    
    add_executable(hello-world hello-world.cpp)

 The CMake language is case *insensitive*, but the arguments are case *sensitive*.

**In CMake, C++ is the default programming language.** However, we suggest to always explicitly state the project’s language in the project command using the LANGUAGES option.

To configure the project and generate its build system, we have to run CMake through its command-line interface (CLI). The CMake CLI offers a number of switches, cmake --help will output to screen the full help menu listing all of the available switches. We will learn more about them throughout the book. As you will notice from the output of **cmake --help, most of them will let you access the CMake manual.** The typical series of commands issued for generating the build system is the following:

    $ mkdir -p build
    $ cd build
    $ cmake ..

Here, we created a directory, build, where the build system will be generated, we entered the build directory, and invoked CMake by pointing it to the location of CMakeLists.txt (in this case located in the parent directory). It is possible to use the following invocation to achieve the same effect:

    $ cmake -H. -Bbuild

This invocation is cross-platform and introduces the -H and -B CLI switches. With **-H. we are instructing CMake to search for the root CMakeLists.txt file in the current directory**

**-Bbuild tells CMake to generate all of its files in a directory called build.**



Note that the cmake -H. -Bbuild invocation of CMake is still undergoing standardization: https://cmake.org/pipermail/cmake-developers/2018-January/030520.html. This is the reason why we will instead use the traditional approach in this book (create a build directory, step into it, and configure the project by pointing CMake to the location of CMakeLists.txt).



Running the cmake command outputs a series of status messages to inform you of the configuration:

    $ cmake ..
    
    -- The CXX compiler identification is GNU 8.1.0
    -- Check for working CXX compiler: /usr/bin/c++
    -- Check for working CXX compiler: /usr/bin/c++ -- works
    -- Detecting CXX compiler ABI info
    -- Detecting CXX compiler ABI info - done
    -- Detecting CXX compile features
    -- Detecting CXX compile features - done
    -- Configuring done
    -- Generating done
    -- Build files have been written to: /home/user/cmake

Running cmake . in the same directory as CMakeLists.txt would in principle be enough to configure a project. However, CMake would then write all generated files into the **root** of the project. **This would be an *in-source build* and is generally undesirable, as it mixes the source and the build tree of the project**. The *out-of-source* build we have demonstrated is the preferred practice.



CMake is a build system *generator*. You describe what type of operations the build system, such as Unix Makefiles, Ninja, Visual Studio, and so on, will have to run to get your code compiled. In turn, CMake *generates* the corresponding instructions for the chosen build system. By default, on GNU/Linux and macOS systems, CMake employs the Unix Makefiles generator. On Windows, Visual Studio is the default generator. We will take a closer look at generators in the next recipe and also revisit generators in Chapter 13, *Alternative Generators and Cross-compilation*.  
On GNU/Linux, CMake will by default generate Unix Makefiles to build the project:



- Makefile: The set of instructions that make will run to build the project.
- CMakeFiles: Directory which contains temporary files, used by CMake for detecting the operating system, compiler, and so on. In addition, depending on the chosen *generator,* it also contains project-specific files.
- cmake_install.cmake: A CMake script handling install rules, which is used at install time.
- CMakeCache.txt: The CMake cache, as the filename suggests. This file is used by CMake when re-running the configuration.



To build the example project, we ran this command:



    cmake --build .

This command is a generic, cross-platform wrapper to the native build command for the chosen *generator*, make in this case. **We should not forget to test our example executable:**

    $ ./hello-world
    
    Hello, CMake world!

Finally, we should point out that CMake does not enforce a specific name or a specific location for the build directory. We could have placed it completely outside the project path. This would have worked equally well:



    $ mkdir -p /tmp/someplace
    $ cd /tmp/someplace
    $ cmake /path/to/source
    $ cmake --build .


