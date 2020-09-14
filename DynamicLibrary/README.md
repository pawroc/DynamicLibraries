# Introduction

This project is made as some skeleton for dynamic libraries projects.
It is meant mainly for Linux distributions but it could be developed in future
for different platforms as well.

## CMake

This project bases on CMake as it is great buildscripts generator tool.

## Build

### Default

1. Jump to root directory of the project
2. Create `build` directory: `mkdir build`
3. Jump to `build` directory
4. Generate buildscripts
```bash
cmake ..
```
5. Build project using chosen generator, i.e. `make`
```bash
make
```

### Custom build

You can custom your build by passing some values to cmake, e.g.:
- `cmake -DCMAKE_INSTALL_PREFIX=<your_prefix>` - this will change default `/user/local` install prefix
- `cmake -DBUILD_SHARED_LIBS=OFF` - this will force all libraries of not specified type to be of `SHARED` type

## Install

1. Generate buildscripts as described in 1-4 steps in __BUILD__ section
2. Execute install command: `make install`

__NOTE__: all files will be installed to standard directories defined by CMake:
- `CMAKE_INSTALL_LIBDIR` - for `ARCHIVE` and `LIBRARY`
- `CMAKE_INSTALL_BINDIR` - for `RUNETIME`
- `CMAKE_INSTALL_INCLUDEDIR` - for API include directory

All paths are prefixed by `CMAKE_INSTALL_PREFIX`.
