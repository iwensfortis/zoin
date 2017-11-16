# Guide for cross compiling Zoin win x32 on ubuntu #
## Tested on Ubuntu 16.04.2 LTS ##

Start on ubuntu your terminal (search for terminal with ubuntu dash top icon left, or press windows key to open dash).
We need to install the dependencies for MXE, for this execute the following cmd’s stated below in the terminal window. 
The commands to execute are in the grey textboxes. 
Some commands will ask for your password, and u have to confirm some commands with y from yes. 

First update the apt library.

```
sudo apt-get update
```

Install needed dependencies to install mxe to cross compile zoin.

```
sudo apt-get install p7zip-full autoconf automake autopoint bash bison bzip2 cmake flex gettext git \
g++ gperf intltool libffi-dev libtool libltdl-dev libssl-dev libxml-parser-perl make openssl patch perl \
pkg-config python ruby scons sed unzip wget xz-utils libtool-bin libgdk-pixbuf2.0-dev g++-multilib \
libc6-dev-i386 upx -y
```

We need to get latest mxe from github and compile the needed libraries.

```
git clone https://github.com/mxe/mxe.git
```
```
cd mxe
```
```
make MXE_TARGETS="i686-w64-mingw32.static" boost
```
```
make MXE_TARGETS="i686-w64-mingw32.static" qttools
```
```
make MXE_TARGETS="i686-w64-mingw32.static" miniupnpc
```
```
make MXE_TARGETS="i686-w64-mingw32.static" libqrencode
```

Next we need to compile the recommended Berkeley DB our self

```
cd ~
```
```
wget 'http://download.oracle.com/berkeley-db/db-4.8.30.NC.tar.gz'
```
```
tar zxvf db-4.8.30.NC.tar.gz
```
```
cd db-4.8.30.NC
```

Now we will create a bash script for this  execute the following commands to create the bash script.

```
touch compile-db.sh
```
```
chmod ugo+x compile-db.sh
```
```
nano compile-db.sh
```

Copy and paste the following code in the window that opened after the last commando.

```
#!/bin/bash
MXE_PATH=$HOME/mxe
sed -i "s/WinIoCtl.h/winioctl.h/g" src/dbinc/win_db.h
mkdir build_mxe
cd build_mxe
 
CC=$MXE_PATH/usr/bin/i686-w64-mingw32.static-gcc \
CXX=$MXE_PATH/usr/bin/i686-w64-mingw32.static-g++ \
../dist/configure \
                --disable-replication \
                --enable-mingw \
                --enable-cxx \
                --host x86 \
                --prefix=$MXE_PATH/usr/i686-w64-mingw32.static

make 2>&1 | tee ~/make-db-debug.txt 

make install
```

Press the following keyboard keys to save the file.
*	Ctrl o
*	Enter
*	Ctrl x

Then execute the following Command to start compiling the berkeley database 

```
./compile-db.sh
```

Finally we need to compile zoin, for this we will pull the last code from github and create another compile script.

```
cd ~
```
```
git clone https://github.com/zoinofficial/zoin
```
```
cd zoin
```
```
touch compile-zoi.sh
```
```
chmod ugo+x compile-zoi.sh
```
```
nano compile-zoi.sh
```

Copy and paste the following code in the window that opened after the last command.

```
#!/bin/bash
MXE_PATH=$HOME/mxe
 
MXE_INCLUDE_PATH=$MXE_PATH/usr/i686-w64-mingw32.static/include
MXE_LIB_PATH=$MXE_PATH/usr/i686-w64-mingw32.static/lib
INCLUDEPATH=$MXE_PATH/zoin/build
 
$MXE_PATH/usr/bin/i686-w64-mingw32.static-qmake-qt5 \
                BOOST_LIB_SUFFIX=-mt \
                BOOST_THREAD_LIB_SUFFIX=_win32-mt \
                BOOST_INCLUDE_PATH=$MXE_INCLUDE_PATH \
                BOOST_LIB_PATH=$MXE_LIB_PATH \
                OPENSSL_INCLUDE_PATH=$MXE_INCLUDE_PATH/openssl \
                OPENSSL_LIB_PATH=$MXE_LIB_PATH \
                BDB_INCLUDE_PATH=$MXE_INCLUDE_PATH \
                BDB_LIB_PATH=$MXE_LIB_PATH \
                MINIUPNPC_INCLUDE_PATH=$MXE_INCLUDE_PATH \
                MINIUPNPC_LIB_PATH=$MXE_LIB_PATH \
                QMAKE_LRELEASE=$MXE_PATH/usr/i686-w64-mingw32.static/qt5/bin/lrelease zoin.pro
 
TARGET_OS=NATIVE_WINDOWS make CC=$MXE_PATH/usr/bin/i686-w64-mingw32.static-gcc \
CXX=$MXE_PATH/usr/bin/i686-w64-mingw32.static-g++ 2>&1 | tee make-w32.static-zoi-debug.txt
```

Press the following keyboard keys to save the file.

*	Ctrl o
*	Enter
*	Ctrl x

Now execute the last 2 commands to start compiling (if u close the terminal u will have to execute both cmd’s again)

```
export PATH=$HOME/mxe/usr/bin:$PATH
```
```
./compile-zoi.sh
```

After some time the zoin-qt.exe will be in the zoin dir in your home folder.
