---
title: CMake
last-changed: <time>2018-05-02</time>
knowledgebase: true
categories: [Programming]
---
## Links

* [CMake Tutorial](http://www.cmake.org/cmake/help/cmake_tutorial.html) <time>2018-05-02</time>
* [CMake Wiki](https://gitlab.kitware.com/cmake/community/wikis/home) <time>2018-05-02</time>

## Usage

``` sh
$ mkdir build
$ cd build
$ cmake -DCMAKE_INSTALL_PREFIX=$HOME/local/usr ..
$ cmake -DCMAKE_PREFIX_PATH=$HOME/local/usr ..
```

### Variablen

``` text
CMAKE_INSTALL_PREFIX
CMAKE_PREFIX_PATH
```
