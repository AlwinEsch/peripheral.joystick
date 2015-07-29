# Building out-of-tree (recommended)

## Linux

Create and enter a build directory

```shell
mkdir peripheral.joystick
cd peripheral.joystick
```

Generate a build environment with config for debugging

```shell
cmake -DADDONS_TO_BUILD=peripheral.joystick \
      -DCMAKE_BUILD_TYPE=Debug \
      -DCMAKE_INSTALL_PREFIX=$HOME/workspace/xbmc/addons \
      -DPACKAGE_ZIP=1 \
      $HOME/workspace/xbmc/project/cmake/addons
```

The add-on can then be built with `make`.

# Building stand-alone (development)

Stand-alone builds are closer to "normal" software builds. The build system looks for its dependencies, by default with `/usr` and `/usr/local` prefixes.

To provide these dependencies yourself in a local working directory (`$HOME/kodi`), build Kodi with an installation prefix

```shell
./configure --prefix=$HOME/kodi
make
make install
```

Clone kodi-platform and create a CMake build directory

```shell
git clone https://github.com/xbmc/kodi-platform.git
cd kodi-platform
mkdir build
cd build
cmake -DCMAKE_BUILD_TYPE=Debug \
      -DCMAKE_INSTALL_PREFIX=$HOME/kodi \
      ..
make
make install
```

The platform library was split from kodi-platform. Do the same as above for the new platform library:

```
git clone https://github.com/Pulse-Eight/platform.git
...
```

With these dependencies in place, the add-on can be built. Use the CMake command above, or use this command to copy add-ons to your Kodi source tree:

```shell
cmake -DCMAKE_BUILD_TYPE=Debug \
      -DCMAKE_PREFIX_PATH=$HOME/kodi \
      -DCMAKE_INSTALL_PREFIX=$HOME/workspace/xbmc/addons \
      -DPACKAGE_ZIP=1 \
      ..
```

# Building in-tree (cross-compiling)

Kodi's build system will fetch the add-on from the GitHub URL and git hash specified in [peripheral.joystick.txt](https://github.com/garbear/xbmc/blob/retroplayer-15alpha2/project/cmake/addons/addons/peripheral.joystick/peripheral.joystick.txt).

## Linux

Ensure that kodi has been built successfully. Then, from the root of the source tree, run

```shell
make install DESTDIR=$HOME/kodi
```

Build the add-on

```shell
make -C tools/depends/target/binary-addons PREFIX=$HOME/kodi ADDONS="peripheral.joystick"
```

The compiled .so can be found at

```
$HOME/kodi/lib/kodi/addons/peripheral.joystick/peripheral.joystick.so
```

To rebuild the add-on or compile a different one, clean the build directory

```shell
make -C tools/depends/target/binary-addons clean
```

## Windows

First, download and install [CMake](http://www.cmake.org/download/).

To compile on windows, open a command prompt at `tools\buildsteps\win32` and run the script:

```
make-addons.bat install peripheral.joystick
```

Alternatively, generate the `kodi-addons.sln` Visual Studio solution and project files.

```
tools\windows\prepare-binary-addons-dev.bat
```

The generated solution can be found at

```
project\cmake\addons\build\kodi-addons.sln
```

Add-ons can be built individually through their specific project, or all at once by building the solution.

No source code is downloaded at the CMake stage; when the project is built, the add-on's source will be downloaded and compiled.

## OSX

Per [README.osx](https://github.com/garbear/xbmc/blob/retroplayer-15alpha2/docs/README.osx), enter the `tools/depends` directory and make the add-on:

```shell
cd tools/depends
make -C target/binary-addons ADDONS="peripheral.joystick"
```

To rebuild the add-on or compile a different one, clean the build directory

```shell
make -C target/binary-addons clean
```
