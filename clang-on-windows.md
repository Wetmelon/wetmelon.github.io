
# Using Clang on Windows for C++
Table of Contents
- [Using Clang on Windows for C++](#using-clang-on-windows-for-c)
  - [Option 1: Link to Microsoft's STL using MSVC](#option-1-link-to-microsofts-stl-using-msvc)
  - [Option 2: Link to libstdc++ using gcc / g++](#option-2-link-to-libstdc-using-gcc--g)

---

Clang can be used on Windows, and LLVM provides releases on their website: https://releases.llvm.org/download.html

1. Download the Windows (64-bit) package
2. Install LLVM and add to PATH
3. Open powershell and type `clang++ --version`.  It should report the version.

Clang is now installed, but isn't very useful yet.  On Windows, you can now compile with Clang, but LLVM doesn't ship with a standard library.  Luckily, we have a couple of choices on Windows.

## Option 1: Link to Microsoft's STL using MSVC
By default, Clang uses the target `x86_64-pc-windows-msvc` when compiling on Windows.  With this target, Clang will look for Microsoft's `msvc` linker on PATH and use it for linking.  If you want to go that route, install [Microsoft Visual Studio](https://visualstudio.microsoft.com/downloads/).

One step compile & link: 

    clang++ main.cpp -o myprogram.exe

Two-step compile & link:

    clang++ -c main.cpp -o main.o
    clang++ main.o -o myprogram.exe

## Option 2: Link to libstdc++ using gcc / g++
It is possible to use `gcc`'s linker and `libstdc++` implementation when compiling with Clang on Windows.  First, you will have to install gcc.  I currently recommend the [TDM-gcc builds](https://jmeubank.github.io/tdm-gcc/).  Download and install using the default settings. When compiling, you have to pass some new flags to let clang know that you want to use the "gnu compiler collection" (gcc) instead of `msvc`.

One step compile & link: 

    clang++ -target x86_64-pc-windows-gnu main.cpp -o myprogram.exe

Two-step compile & link:

    clang++ -target x86_64-pc-windows-gnu -c main.cpp -lpthread -o main.o
    clang++ -target x86_64-pc-windows-gnu main.o -lpthread -o myprogram.exe

Notice that I am using a different target triple, and linking the `pthread` library.  `-lpthread` must come after the list of objects to be linked (after `main.o` in this case).

That's it!  Happy compiling!

\- Wetmelon

---
Oct 29, 2020