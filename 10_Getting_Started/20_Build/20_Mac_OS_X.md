**_It is advised to read through this entire doc page before getting started_**

Prerequisites for OS X
======================

### Compiler

SOFA requires at least GCC 4.8 or Clang 3.4. To make sure you have a correct version, execute the usual
commands:
```bash
gcc --version
clang --version
```

### Compiler: LLVM vs. GCC

The default compiler on MacOS is now LLVM (ie. clang). SOFA is compatible with Clang but if you want to use gcc/g++ instead it is
necessary to tell CMake.

To change the default compiler, set `CMAKE_C_COMPILER` and `CMAKE_CXX_COMPILER` in cmake.
For example, to set the compiler to gcc through command line :
`cmake -DCMAKE_C_COMPILER=llvm-gcc -DCMAKE_CXX_COMPILER=llvm-g++ ...`
You may require to regenerate the project for the changes to take effect.

WARNING: if you are on MacOS 10.8 Mountain Lion, *gcc* is an alias of *clang*. To really
use gcc, use `CC=llvm-gcc` and `CXX=llvm-g++`.


### CMake: Makefile generator

SOFA requires at least CMake 3.1. To install CMake, download the latest DMG package from [their download page](https://cmake.org/download/).


### [optional] Ninja: build system

Ninja is an alternative to Make. It has a better handling of incremental builds.

``` {.bash .optional}
brew install ninja
```

To use Ninja, do not forget to set the CMake generator to "Codeblocks - Ninja" (as explained in [Generate a Makefile with CMake](#generate-a-makefile-with-cmake)).


Required dependencies
=====================

To compile SOFA, you need to install several dependencies using Homebrew

-   **Qt** (>= 5.5.0)

    We recommend to install Qt **in your user directory** with [the unified installer](http://download.qt.io/official_releases/online_installers).  

-   **Boost** (>= 1.54.0)

    ```bash
    brew install boost
    ```

-   **Additional libraries**: libPNG, libTIFF, libJPEG, Zlib, Glew

    ```bash
    brew install libpng libjpeg libtiff glew
    ```


Building on OS X
================


## Setting up your source and build directories

To set up clean repositories, we propose to arrange the SOFA directories
as follows:

-   sofa/
    -   src/
    -   build/
        -   v19.06/
        -   master/

**First**, download the sources from Git repository:

Get the current **stable** version on the v19.06 branch:
``` {.bash .stable}
git clone -b v19.06 https://github.com/sofa-framework/sofa.git sofa/src
```

**OR** get the development **unstable** version on the master branch:
``` {.bash .unstable}
git clone -b master https://github.com/sofa-framework/sofa.git sofa/src
```


## Generate a Makefile with CMake

If you didn't do it yet, create a build/ folder respecting directories
arrangement.

Run CMake-GUI and set source folder with **Browse Source** and build
folder with **Browse Build**.

Next, run **Configure**. A popup window will ask you to specify the
generator for the project.  
If you installed Ninja, select "Codeblocks - Ninja". Otherwise, select "Codeblocks - Makefile".  
Keep "Use default native compilers" selected, and press "Finish".

You need to **run Configure twice**, since SOFA requires two passes to
manage the module dependencies. You can then customize your version of
SOFA, activate or deactivate plugins and functionalities.

When you are ready, press **Generate**. This will create your Visual
Studio solution or your makefiles if you chose another generator.

#### Troubleshooting: Qt detection errors

To solve Qt detection errors, click on **Add Entry** and add
`CMAKE_PREFIX_PATH` with path `/Users/YOUR_USERNAME/Qt/QT_VERSION/COMPILER` matching your
Qt architecture.  
Example: `CMAKE_PREFIX_PATH=/Users/bob/Qt/5.7/clang_64`  
**Configure** again.

A further dev warning may appear:

    CMake Warning (dev) at YOUR_QT_PATH/lib/cmake/Qt5Core/Qt5CoreMacros.cmake:224 (configure_file):
    configure_file called with unknown argument(s):

    COPY_ONLY

    Call Stack (most recent call first):
    applications/projects/Modeler/exec/CMakeLists.txt:14 (qt5_add_resources)

This is just a typo with Qt5CoreMacros.cmake file. It uses COPY\_ONLY
instead of COPYONLY. Simply edit your Qt5CoreMacros.cmake, replace
COPY\_ONLY with COPYONLY and **Configure** again.


## Compile

To compile in the terminal, go to your build directory and run `make` or `ninja` depending on the generator you chose during CMake configuration.  
Do not forget the `-j` option to use all your CPU cores.

Time for a coffee!



## Setting up QtCreator

The following instructions assume that you have set up two build
directories as explained in the previous section.

In QtCreator, open project CMakeLists.txt. Choose build-release as build
directory, then click on Finish. QtCreator is ready to compile, and the
build configuration is named "all", though it corresponds to a Release.

Click on the Project button in the left, then rename the configuration
from "all" to "Release".

Now click on add/Build to create a new configuration called "Debug".
Choose build-debug as build directory and run Cmake. If you set the -G
"CodeBlocks - Unix Makefiles" option in the cmake command line discussed
in the previous section, you do not even need to run CMake.

You can now switch between Debug and Release in QtCreator. The
compilation will be done using ccache (if installed). You can check this
by setting VERBOSE=1 as additional argument to Make in the Projects tab
on the left. Moreover, you probably want to run parallel compilations by
setting *-j10* for instance, for 10 parallel compilations, as additional
argument to Make. Ubuntu 14.04: default qtcreator cmake plugin is
bugged, but you can easily install qtcreator 3.1.1 from this
non-official repository
[\[1\]](https://launchpad.net/~alexey-ivanov/+archive/qtcreator "https://launchpad.net/~alexey-ivanov/+archive/qtcreator"){.external
.autonumber} (Warning, it is using qt4 rather than qt5).

