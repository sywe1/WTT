<img src="https://github.com/uvic-aurora/WTT/blob/master/src/wtt-demo/screenshots/screenshots.gif" width="850" alt="DemoScreenshot" />

Wavelet Transform Toolkit (WTT)
=========================

[![Build Status](https://travis-ci.com/uvic-aurora/WTT.svg?branch=master)](https://travis-ci.com/uvic-aurora/WTT)

Wavelet Transform Toolkit (WTT) consists of a C++ header-only library for computing and defining multi-level lifted wavelet transforms on 3-D meshes. In addition to the library, the author has implemented a Qt- and OpenGL-based demo program for viewing the result of computing wavelet transforms on a 3-D mesh and demonstrating wavelet-based compression and denoising. Threaded rendering has been employed to provide a smooth user experience. Also, WTT consists of several command-line programs for simple use cases (e.g., computing wavelet transforms, wavelet denoising, and wavelet compression).

The author Shengyang Wei can be reached at the following email address:

* shengyangwei@protonmail.com
* Github: [sywe1](https://github.com/sywe1)

Dependency
------------

A complier with C++ 17 support is needed to compile WTT. The following versions of compilers and depended libraries have been verified to work with WTT:

* g++ (7.2+) or clang++ (5+)
* CGAL (4.2+)
* Boost (1.58+)

Since the demo is based on Qt and OpenGL, they are needed to build the demo program of WTT. The following versions of Qt and OpenGL have been verified to work with WTT:

* Qt (5.9+)
* OpenGL (3.3+)

In case the demo program is not needed, users could set the cmake option `BUILD_DEMO` to `OFF` (default: `ON`) to skip building the demo while running the following installation command. In this case, Qt and OpenGL are no longer need to be installed.

Installation
------------

In what follows, let `$TOP_DIR` denote the top-level directory of WTT software distribution (i.e., the directory containing this README file); let `$BUILD_DIR` denote a new directory to be created for building the software; and let `$INSTALL_DIR` denote the directory in which to install WTT.

To install WTT with CMake, use the command sequence:

```shell
cmake -H$TOP_DIR -B$BUILD_DIR -DCMAKE_INSTALL_PREFIX=$INSTALL_DIR
cmake --build $BUILD_DIR --target install
```

Usage of the Demo Program
-----------------------------

After a successful installation, the demo program can be found in `$INSTALL_DIR/bin`. To launch the program, users could run the following command:

```shell
./wtt_demo
```

The demo is designed to provide an instant view of computing the forward wavelet transform and inverse wavelet transform. Both Loop and Butterfly wavelet transforms are supported. Users could also try wavelet compression and denoising by performing a forward wavelet transform, modifying wavelet coefficients, and performing an inverse wavelet transform. The buttons on the bottom panel provide these wavelet related functionalities, e.g., loading a mesh, computing a forward and inverse wavelet transform, and filtering wavelet coefficients. The buttons on the left panel are used to control the OpenGL view and switch shaders. When the mouse hovers on a button, a helper text will be displayed to illustrate the action of the button.

Usage of Command-line Programs
------------------------------

After a successful installation, three programs can be found in directory `$INSTALL_DIR/bin`: `wtt_fwt`, `wtt_iwt`, and `wtt_filter`. Programs `wtt_fwt` and `wtt_iwt` are used to compute the forward and inverse wavelet transform on a triangle mesh, respectively. Program `wtt_filter` combines the forward and inverse transform and filters the intermediately produced wavelet coefficients. Users could set the filtering schemes via command-line options to perform wavelet denoising and compression on a triangle mesh. For the three programs, either the Loop or Butterfly scheme can be selected via command-line options.
Recently, the supported file format for a 3-D mesh is OFF.

Some example usages are listed below:

* To compute the 2-level Loop forward wavelet transform on mesh `vase-8.off`, users could run the following command:

```shell
wtt_fwt -m Loop -l 2 -i vase-8.off -o vase-fwt.off -c vase-fwt.coefs
```

Mesh `vase-8.off` could be found in `$INSTALL_DIR/data` directory.

* To recover the original `vase.off` by 2-level Loop inverse wavelet transform, users could run the following command:

```shell
wtt_iwt -m Loop -l 2 -i vase-fwt.off -c vase-fwt.coefs -o vase-recover.off
```

* To perform 3-level Butterfly wavelet compression on mesh `vase-8.off`, users could run the following command:

```shell
wtt_filter -m Butterfly -l 3 -i vase-8.off -o vase-reconstructed.off -c 5
```

The above command compresses the wavelet coefficients to 5%. That is, only the 5% wavelet coefficients are used to construct the output mesh.

Usage of Library API
---------------------

The API is provided for defining and computing a custom wavelet transform. Users could incorporate custom operations into the API to define a lifted wavelet transform. If PTQ is used in the wavelet transform, some built-in functions of the WTT could be used to accelerate the development. The code example below with line-by-line comments illustrate the steps of defining a custom wavelet transform.

* [defining a lifted wavelet transform](examples/define_a_wt_with_custom_lifting_steps.cpp)

In case the Loop and Butterfly wavelet transforms are of interest, the WTT library provides built-in functions that can be called directly for computing those wavelet transforms. The examples below illustrate the usage of the two built-in functions for the Loop and Butterfly wavelet transform.

* [computing loop wavelet transform](examples/usage_of_loop_wavelet_transform.cpp)
* [computing butterfly wavelet transform](examples/usage_of_butterfly_wavelet_transform.cpp)
