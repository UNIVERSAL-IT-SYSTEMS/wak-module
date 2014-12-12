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
