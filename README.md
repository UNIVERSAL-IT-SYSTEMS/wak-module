wak-module
==========

Common repository for [Wakanda](http://www.wakanda.org) module development.

Building a mode
-----

**Ubuntu**

Most open-source projects written in C/C++ can be built for the Linux platform with the 3 lines

```sh
./configure
make
sudo make install
```

However, to make System Worker modules more protable, I prefer to build shared libraries that search dependencies in the current directory.

To ensure that the executable has a [runtime seach path](http://en.wikipedia.org/wiki/Rpath), define the following 2 environemnt variables. 

```sh
export LD_LIBRARY_PATH='/usr/local/lib:/usr/lib'
export LD_RUN_PATH='/usr/local/lib:/usr/lib'
```
Also check if the "configure" script supports the option "--enable-rpath".

After the console program is built, check that it has an RPATH.

```sh
readelf -d
```

Replace the runtime search path with $ORIGIN.

```sh
chrpath -r '$ORIGIN' 
```

**Mac OS X**

Some "configure" scripts wrongly assume a 32 bit platform. To make sure you build 64 bits, set some compiler flags.

```sh
export CCFLAGS="-arch x86_64"
export CXXFLAGS="-arch x86_64"
```

Some cross-platform libraries use system SDKs. If your system is the latest Mac, you might want to link to a lower SDK version, for users running an older version of OS X.

```sh
export CFLAGS="-arch x86_64 -isysroot /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.8.sdk -mmacosx-version-min=10.8"
export LDFLAGS="-arch x86_64 -isysroot /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.8.sdk -mmacosx-version-min=10.8"
export CPPFLAGS="-arch x86_64 -isysroot /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.8.sdk -mmacosx-version-min=10.8"
```
Some scripts fail to find dependencies.

```sh
export LDFLAGS="-L/usr/local/lib"
export CPPFLAGS="-I/usr/local/include"
```
After the console program is built, check its install name and dependencies.
```sh
OTOOL -L {path}
```
Change its install name to be relative to the executable. 

```sh
install_name_tool -id @loader_path/{name} path
```

Do the same for all libraries.

```sh
install_name_tool -change {library_path} @loader_path/{library_name} path
```
