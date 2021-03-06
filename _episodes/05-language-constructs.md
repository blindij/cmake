---
layout: episode
title: "Branching, variables, lists, and loops"
teaching: 10
exercises: 0
questions:
  - Is CMake a full-fledged programming language?
objectives:
  - Learn how to set and print variables.
  - Learn how to avoid code repetition.
keypoints:
  - CMake allows you to structure your code to avoid code repetition.
---

## Branching

Note the unusual parentheses in `else()` and `endif()`:

```cmake
if(CURRENT_WEATHER STREQUAL "sunny day")
    message("Today is a ${CURRENT_WEATHER}. Let us go for a walk.")
else()
    message("Better stay inside the house.")
endif()
```

Write this to a CMakeLists.txt and execute cmake in a subdirectory, like this:

```shell
$ mkdir build
$ cd build
$ cmake ..

-- The C compiler identification is AppleClang 7.3.0.7030031
-- The CXX compiler identification is AppleClang 7.3.0.7030031
-- Check for working C compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/cc
-- Check for working C compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/c++
-- Check for working CXX compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
Better stay inside the house.
-- Configuring done
-- Generating done
-- Build files have been written to: /Users/bjornlin/tmp/cmake/build
```

Let us try the other branch by setting the variable ${CURRENT_WEATHER}. Here from the command line:

```shell
$ cmake -DCURRENT_WEATHER="sunny day" ..
Today is a sunny day. Let us go for a walk.
-- Configuring done
-- Generating done
-- Build files have been written to: /Users/bjornlin/tmp/cmake/build

```

## Messages to the user (or to you)

Useful for debugging CMake files:

```cmake
# display message
message("We are right here.")

# display STATUS message with a -- in the command line
message(STATUS "Still everything under control ...")

# display message and halt configuration
message(FATAL_ERROR "Something unexpected happened!")
```

---

## Variables, lists, and loops

```cmake
set(CURRENT_WEATHER "sunny day")

message("We'll meet again some ${CURRENT_WEATHER} ...")

set(FRUITS apple banana orange kiwi mango)

foreach(fruit ${FRUITS})
    message("${fruit} is a tasty fruit")
endforeach()
```

You can append to sets like this:

```cmake
set(FRUITS apple banana orange kiwi mango)

# we add grapes to the list of fruits
set(FRUITS ${FRUITS} grapes)
```

---

## Functions and macros

Functions are like C/C++ functions, and arguments are available by their
argument names or from their argument position (ARGV0, ARGV1, ...):

```cmake
function(my_function firstname lastname)
    message("called my_function with the arguments: '${firstname}' and '${lastname}'")
    message("The number of arguments is ${ARGC}")
    message("ARGV0: ${ARGV0} ARGV1: ${ARGV1}")
    set(MY_VARIABLE "${lastname} ${firstname}")  # this will not be visible outside
endfunction()

macro(my_macro firstname lastname)
    message("called my_macro with the arguments: '${firstname}' and '${lastname}'")
    set(MY_VARIABLE "${lastname} ${firstname}")  # this will be visible outside
endmacro()

my_function(James Bond)
message("MY_VARIABLE set to: '${MY_VARIABLE}'")

my_macro(James Bond)
message("MY_VARIABLE set to: '${MY_VARIABLE}'")
```

Function variables have function scope:

```shell
called my_function with the arguments: 'James' and 'Bond'
The number of arguments is 2
ARGV0: James ARGV1: Bond
MY_VARIABLE set to: ''

called my_macro with the arguments: 'James' and 'Bond'
MY_VARIABLE set to: 'Bond James'
```

Question: should you rather use named arguments or numbered arguments?
