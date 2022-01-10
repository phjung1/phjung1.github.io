---
layout: post
title: Building and linking static and shared libraries
date: 2022-01-10 01:27:00
author: "phjung1"
header-img: "#"
tags:

- Cmake

---

# Building and linking static and shared libraries

The code for this recipe is available at [cmake-cookbook/chapter-01/recipe-03 at v1.0 · dev-cafe/cmake-cookbook · GitHub](https://github.com/dev-cafe/cmake-cookbook/tree/v1.0/chapter-01/recipe-03) and has a C++ and Fortran example. The recipe is valid with CMake version 3.5 (and higher) and has been tested on GNU/Linux, macOS, and Windows.

A project almost always consists of more than a single executable built from a single source file. Projects are split across multiple source files, often spread across different subdirectories in the source tree. This practice not only helps in keeping source code organized within a project, but greatly favors modularity, code reuse, and separation of concerns, since common tasks can be grouped into libraries. This separation also simplifies and speeds up recompilation of a project during development. In this recipe, we will show how to group sources into libraries and how to link targets against these libraries.

## Getting ready

Let us go back to our very first example. However, instead of having one single source file for the executable, we will now introduce a class to wrap the message to be printed out to screen. This is our updated hello-world.cpp:

```cpp
#include 'Message.hpp'

#include <cstdlib>
#include <iostream>

int main() {
  Message say_hello('Hello, CMake World!');

  std::cout << say_hello << std::endl;

  Message say_goodbye('Goodbye, CMake World');

  std::cout << say_goodbye << std::endl;

  return EXIT_SUCCESS;
}
```

The Message class wraps a string, provides an overload for the << operator, and consists of two source files: the Message.hpp header file and the corresponding Message.cpp source file. The Message.hpp interface file contains the following:

```cpp
#pragma once

#include <iosfwd>
#include <string>

class Message {
public:
  Message(const std::string &m) : message_(m) {}

  friend std::ostream &operator<<(std::ostream &os, Message &obj) {
    return obj.printObject(os);
  }

private:
  std::string message_;
  std::ostream &printObject(std::ostream &os);
};
```

The corresponding implementation is contained in Message.cpp:

```cpp
#include 'Message.hpp'

#include <iostream>
#include <string>

std::ostream &Message::printObject(std::ostream &os) {
  os << 'This is my very nice message: ' << std::endl;
os << message_;

  return os;
}
```

## How to do it

These two new files will also have to be compiled and we have to modify CMakeLists.txt accordingly. However, in this example we want to compile them first into a library, and not directly into the executable:

1. Create a new *target*, this time a static library. The name of the library will be the name of the target and the sources are listed as follows:
   
    add_library(message 
      STATIC
   
        Message.hpp
        Message.cpp
   
      )

2. The creation of the target for the hello-world executable is unmodified:
   
    add_executable(hello-world hello-world.cpp) 

3. Finally, tell CMake that the library target has to be linked into the executable target:
   
    target_link_libraries(hello-world message)

4. We can configure and build with the same commands as before. This time a library will be compiled, alongside the hello-world executable:
   
    $ mkdir -p build
    $ cd build
    $ cmake ..
    $ cmake --build .
   
    Scanning dependencies of target message
    [ 25%] Building CXX object CMakeFiles/message.dir/Message.cpp.o
    [ 50%] Linking CXX static library libmessage.a
    [ 50%] Built target message
    Scanning dependencies of target hello-world
    [ 75%] Building CXX object CMakeFiles/hello-world.dir/hello-world.cpp.o
    [100%] Linking CXX executable hello-world
    [100%] Built target hello-world
   
    $ ./hello-world
   
    This is my very nice message: 
    Hello, CMake World!
    This is my very nice message: 
    Goodbye, CMake World

## How it works

- add_library(message STATIC Message.hpp Message.cpp): This will generate the necessary build tool instructions for compiling the specified sources into a library. The first argument to add_library is the name of the target. The same name can be used throughout CMakeLists.txt to refer to the library. The actual name of the generated library will be formed by CMake by adding the prefix lib in front and the appropriate extension as a suffix. The library extension is determined based on the second argument, STATIC or SHARED, and the operating system.
- target_link_libraries(hello-world message): Links the library into the executable. This command will also guarantee that the hello-world executable properly depends on the message library. We thus ensure that the message library is always built before we attempt to link it to the hello-world executable.

After successful compilation, the build directory will contain the libmessage.a static library (on GNU/Linux) and the hello-world executable.

CMake accepts other values as valid for the second argument to add_library and we will encounter all of them in the rest of the book:

- STATIC, which we have already encountered, will be used to create static libraries, that is, archives of object files for use when linking other targets, such as executables.
- SHARED will be used to create shared libraries, that is, libraries that can be linked dynamically and loaded at runtime. Switching from a static library to a dynamic shared object (DSO) is as easy as using add_library(message SHARED Message.hpp Message.cpp) in CMakeLists.txt. 
- OBJECT can be used to compile the sources in the list given to add_library to object files, but then neither archiving them into a static library nor linking them into a shared object. The use of object libraries is particularly useful if one needs to create both static and shared libraries in one go. We will demonstrate this in this recipe.
- MODULE libraries are once again DSOs. In contrast to SHARED libraries, they are not linked to any other target within the project, but may be loaded dynamically later on. This is the argument to use when building a runtime plugin.

CMake is also able to generate special types of libraries. These produce no output in the build system but are extremely helpful in organizing dependencies and build requirements between targets:

- IMPORTED, this type of library target represents a library located *outside* the project. The main use for this type of library is to model pre-existing dependencies of the project that are provided by upstream packages. As such IMPORTED libraries are to be treated as immutable. We will show examples of using IMPORTED libraries throughout the rest of the book. See also: [cmake-buildsystem(7) &mdash; CMake 3.22.1 Documentation](https://cmake.org/cmake/help/latest/manual/cmake-buildsystem.7.html#imported-targets)[](https://cmake.org/cmake/help/latest/manual/cmake-buildsystem.7.html#imported-targets)
- INTERFACE, this special type of CMake library is similar to an IMPORTED library, but is mutable and has no location. Its main use case is to model usage requirements for a target that is outside our project. We will show a use case for INTERFACE libraries in Recipe 5, *Distributing a project with dependencies as Conda package*, in Chapter 11, *Packaging Projects*. See also: [cmake-buildsystem(7) &mdash; CMake 3.22.1 Documentation](https://cmake.org/cmake/help/latest/manual/cmake-buildsystem.7.html#interface-libraries)
- ALIAS, as the name suggests, a library of this type defines an alias for a pre-existing library target within the project. It is thus not possible to choose an alias for an IMPORTED library. See also: [cmake-buildsystem(7) &mdash; CMake 3.22.1 Documentation](https://cmake.org/cmake/help/latest/manual/cmake-buildsystem.7.html#alias-libraries)

In this example, we have collected the sources directly using add_library. In later chapters, we demonstrate the use of the target_sources CMake command to collect sources, in particular in Chapter 7, *Structuring Projects*. See also this wonderful blog post by Craig Scott: [Enhanced source file handling with target_sources() - Crascit](https://crascit.com/2016/01/31/enhanced-source-file-handling-with-target_sources/) which further motivates the use of the target_sources command.
