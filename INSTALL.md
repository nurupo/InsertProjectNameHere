#Install Instructions

- [Installation](#installation)
    - [Linux](#linux)
    - [OS X](#osx)
      - [Homebrew](#homebrew)
      - [Non-Homebrew](#non-homebrew)
    - [Windows](#windows)

<a name="installation" />
##Installation

<a name="linux" />
###Linux:

Build dependencies:

```bash
sudo apt-get install build-essential libtool autotools-dev automake libconfig-dev ncurses-dev checkinstall check git libswscale-dev libsdl-dev libopenal-dev libopus-dev libvpx-dev yasm
```

On Fedora:

```bash
yum groupinstall "Development Tools"
yum install libtool autoconf automake libconfig-devel ncurses-devel check check-devel
```

Note that `libconfig-dev` should be >= 1.4.

You should get and install [libsodium](https://github.com/jedisct1/libsodium):
```bash
git clone git://github.com/jedisct1/libsodium.git
cd libsodium
git checkout tags/0.4.2
./autogen.sh
./configure && make check
sudo checkinstall --install --pkgname libsodium --pkgversion 0.4.2 --nodoc
sudo ldconfig
cd ..
```


Or if checkinstall is not easily available for your distribution (e.g. Fedora), 
this will install the libs to /usr/local/lib and the headers to /usr/local/include:

```bash
git clone git://github.com/jedisct1/libsodium.git
cd libsodium
git checkout tags/0.4.2
./autogen.sh
./configure
make check
sudo make install
cd ..
```
If your default prefix is /usr/local and you happen to get an error that says "error while loading shared libraries: libtoxcore.so.0: cannot open shared object file: No such file or directory", then you can try running ```sudo ldconfig```. If that doesn't fix it, run:
```
echo '/usr/local/lib/' | sudo tee -a /etc/ld.so.conf.d/locallib.conf
sudo ldconfig
```

You also need recent [FFmpeg](http://git.videolan.org/?p=ffmpeg.git) libraries:
```bash
git clone git://source.ffmpeg.org/ffmpeg.git
cd ffmpeg
git checkout n2.0.2
./configure --prefix=`pwd`/install --disable-programs
make && make install
cd ..
```

Then clone this repo and generate makefile:
```bash
git clone git://github.com/irungentoo/ProjectTox-Core.git
cd ProjectTox-Core
autoreconf -i
./configure --with-dependency-search=`pwd`/../ffmpeg/install
make
sudo make install
```
Advance configure options:
  - --prefix=/where/to/install
  - --with-libsodium-headers=/path/to/libsodium/include/
  - --with-libsodium-libs=/path/to/sodiumtest/lib/
  - --enable-silent-rules less verbose build output (undo: "make V=1")
  - --disable-silent-rules verbose build output (undo: "make V=0")
  - --disable-tests build unit tests (default: auto)
  - --disable-ntox build nTox client (default: auto)
  - --disable-dht-bootstrap-daemon build DHT bootstrap daemon (default: auto)
  - --enable-shared[=PKGS]  build shared libraries [default=yes]
  - --enable-static[=PKGS]  build static libraries [default=yes]

<a name="osx" />
###OS X:

You need the latest XCode with the Developer Tools (Preferences -> Downloads -> Command Line Tools).
The following libraries are required along with libsodium and cmake for Mountain Lion and XCode 4.6.3 install libtool, automake and autoconf. You can download them with Homebrew, or install them manually.

There are no binaries/executables going to /bin/ or /usr/bin/ now. Everything is compiled and ran from the inside your local branch. See [Usage](#usage) below.
<a name="homebrew" />
####Homebrew:
```
brew install libtool automake autoconf libconfig libsodium check
```
Then clone this repo and generate makefile:
```bash
git clone git://github.com/irungentoo/ProjectTox-Core.git
cd ProjectTox-Core
autoreconf -i
./configure
make
make install
```

If execution fails with errors like "dyld: Library not loaded: /opt/tox-im/lib/libtoxcore.0.dylib", you may need to specify libsodium path:

Determine paths:
```
brew list libsodium
```

Configure include and lib folder and build again:
```bash
./configure--with-libsodium-headers=/usr/local/Cellar/libsodium/0.4.5/include/ --with-libsodium-libs=/usr/local/Cellar/libsodium/0.4.5/lib/
make
make install
```

Advance configure options:
  - --prefix=/where/to/install
  - --with-libsodium-headers=/path/to/libsodium/include/
  - --with-libsodium-libs=/path/to/sodiumtest/lib/
  - --enable-silent-rules less verbose build output (undo: "make V=1")
  - --disable-silent-rules verbose build output (undo: "make V=0")
  - --disable-tests build unit tests (default: auto)
  - --disable-ntox build nTox client (default: auto)
  - --disable-dht-bootstrap-daemon build DHT bootstrap daemon (default: auto)
  - --enable-shared[=PKGS]  build shared libraries [default=yes]
  - --enable-static[=PKGS]  build static libraries [default=yes]

<a name="non-homebrew" />
####Non-homebrew:

Grab the following packages:
  * http://www.gnu.org/software/libtool/
  * http://www.gnu.org/software/autoconf/ 
  * http://www.gnu.org/software/automake/
  * https://github.com/jedisct1/libsodium
  * http://www.hyperrealm.com/libconfig/
  * http://check.sourceforge.net/

Uncompress and install them all. Make sure to follow the README as the instructions change, but they all follow the same pattern below:

```bash
./configure
make
sudo make install
```

In your local TOX repository:

Then generate the makefile:
```bash
cd ProjectTox-Core
autoreconf -i
./configure
make
make install
```
Advance configure options:
  - --prefix=/where/to/install
  - --with-libsodium-headers=/path/to/libsodium/include/
  - --with-libsodium-libs=/path/to/sodiumtest/lib/
  - --enable-silent-rules less verbose build output (undo: "make V=1")
  - --disable-silent-rules verbose build output (undo: "make V=0")
  - --disable-tests build unit tests (default: auto)
  - --disable-ntox build nTox client (default: auto)
  - --disable-dht-bootstrap-daemon build DHT bootstrap daemon (default: auto)
  - --enable-shared[=PKGS]  build shared libraries [default=yes]
  - --enable-static[=PKGS]  build static libraries [default=yes]

Do not install them from macports (or any dependencies for that matter) as they get shoved in the wrong directory
(or the wrong version gets installed) and make your life more annoying.

Another thing you may want to install is the latest gcc, this caused me a few problems as XCode from 4.3
no longer includes gcc and instead uses LLVM-GCC, a nice install guide can be found at
http://caiustheory.com/install-gcc-421-apple-build-56663-with-xcode-42

<a name="windows" />
###Windows:

You should install:
  - [MinGW](http://sourceforge.net/projects/mingw/)

When installing MinGW, make sure to select the MSYS option in the installer.
MinGW will install an "MinGW shell" (you should get a shortcut for it), make sure to perform all operations (i.e. generating/running configure script, compiling, etc.) from the MinGW shell.

First download the source tarball from http://download.libsodium.org/libsodium/releases/ and build it.
Assuming that you got the libsodium-0.4.2.tar.gz release:
```cmd
tar -zxvf libsodium-0.4.2.tar.gz
cd libsodium-0.4.2
./configure
make
make install
cd ..
```

You can also use a precompiled win32 binary of libsodium, however you will have to place the files in places where they can be found, i.e. dll's go to /bin headers to /include and libraries to /lib directories in your MinGW shell.

Next, install ProjectTox-Core library, should either clone this repo by using git, or just download a [zip of current Master branch](https://github.com/irungentoo/ProjectTox-Core/archive/master.zip) and extract it somewhere.

Assuming that you now have the sources in the ProjectTox-Core directory:

```cmd
cd ProjectTox-Core
autoreconf -i
./configure
make
make install
```
Advance configure options:
  - --prefix=/where/to/install
  - --with-libsodium-headers=/path/to/libsodium/include/
  - --with-libsodium-libs=/path/to/sodiumtest/lib/
  - --enable-silent-rules less verbose build output (undo: "make V=1")
  - --disable-silent-rules verbose build output (undo: "make V=0")
  - --disable-tests build unit tests (default: auto)
  - --disable-ntox build nTox client (default: auto)
  - --disable-dht-bootstrap-daemon build DHT bootstrap daemon (default: auto)
  - --enable-shared[=PKGS]  build shared libraries [default=yes]
  - --enable-static[=PKGS]  build static libraries [default=yes]

<a name="Clients" />
####Clients:
While [Toxic](https://github.com/tox/toxic) is no longer in core, a list of Tox clients are located in our [wiki](http://wiki.tox.im/client)

