#Libraries you need to download separately and build:

                default path               download
OpenSSL         \openssl-1.0.1m-mgw        http://www.openssl.org/source/
Berkeley DB     \db-4.8.30.NC-mgw          http://www.oracle.com/technology/software/products/berkeley-db/index.html
Boost           \boost-1.47.0-mgw          http://www.boost.org/users/download/
miniupnpc       \miniupnpc-1.6-mgw         http://miniupnp.tuxfamily.org/files/
                 default path               download
OpenSSL          \openssl-1.0.1m-mgw        http://www.openssl.org/source/
Berkeley DB      \db-4.8.30.NC-mgw          http://www.oracle.com/technology/software/products/berkeley-db/index.html
Boost            \boost-1.55.0-mgw          http://www.boost.org/users/download/
miniupnpc        \miniupnpc-1.9.2-mgw       http://miniupnp.tuxfamily.org/files/
QR Encode        \qrencode-3.4.4-mgw        http://fukuchi.org/works/qrencode/

Their licenses:
OpenSSL        Old BSD license with the problematic advertising requirement
Berkeley DB    New BSD license with additional requirement that linked software must be free open source
Boost          MIT-like license
miniupnpc      New (3-clause) BSD license
QRencode       Free Software

Versions used in this release:
OpenSSL      1.0.1b
OpenSSL      1.0.1m
Berkeley DB  4.8.30.NC
Boost        1.47.0
miniupnpc    1.6
Boost        1.55.0
miniupnpc    1.9.2
qrencode     3.4.4




To Compile on Windows we use MingW and QT.

Install MingW
-------------
http://sourceforge.net/projects/mingw/files/Installer/mingw-get-setup.exe/download
Double click to install, keep the checkbox for the GUI checked and make sure to install in C:\MinGW. Press continue. From the MinGW GUI interface, go to all packages -> MYSYS
Right click on the following installations and mark for installation.

msys-base-bin (may highlight other checkboxes which is fine)
msys-autoconf-bin
msys-automake-bin
msys-libtool-bin

Click on Installation -> Apply changes. MinGW will now download the remaining packages. Make sure to remove any previous installs of MinGW before starting. 
Once complete, navigate to C:\MinGW\bin and you should only have a mingw-get application. If you have msys-gcc and msys-w32api you need to delete MinGW and check the correct install packages are selected above.

Download and extract mingw32 to C:\mingw32
http://sourceforge.net/projects/mingw-w64/files/Toolchains%20targetting%20Win32/Personal%20Builds/mingw-builds/4.9.2/threads-posix/dwarf/i686-4.9.2-release-posix-dwarf-rt_v3-rev1.7z/download ...You now need to change the path variables. Go to control panel->system and security->system. Click on advanced system properties->environmental variables. In the top box navigate to PATH and change to 
C:\mingw32\bin;%SystemRoot%\system32;%SystemRoot%;%SystemRoot%\System32\Wbem;%SYSTEMROOT%\System32\WindowsPowerShell\v1.0\




OpenSSL
-------
MSYS shell:
un-tar sources with MSYS 'tar xfz' to avoid issue with symlinks (OpenSSL ticket 2377)
change 'MAKE' env. variable from 'C:\MinGW32\bin\mingw32-make.exe' to '/c/MinGW32/bin/mingw32-make.exe'

cd /c/openssl-1.0.1m-mgw
./config
un-tar sources with MSYS 'tar xfz' to avoid issue with symlinks.
Download OpenSSL https://www.openssl.org/source/openssl-1.0.1m.tar.gz and place in C:\deps. You can put the dependencies in a different folder though this guide will use the C:\deps folder.
Open the MinGW shell at C:\MinGW\msys\1.0\msys.bat. 
MingW shell:
cd /c/deps/
tar xvfz openssl-1.0.1m.tar.gz
cd openssl-1.0.1m
./Configure no-zlib no-shared no-dso no-krb5 no-camellia no-capieng no-cast no-cms no-dtls1 no-gost no-gmp no-heartbeats no-idea no-jpake no-md2 no-mdc2 no-rc5 no-rdrand no-rfc3779 no-rsax no-sctp no-seed no-sha0 no-static_engine no-whirlpool no-rc2 no-rc4 no-ssl2 no-ssl3 mingw
make



Berkeley DB
-----------
MSYS shell:
cd /c/db-4.8.30.NC-mgw/build_unix
sh ../dist/configure --enable-mingw --enable-cxx
Download http://download.oracle.com/berkeley-db/db-4.8.30.NC.tar.gz and place in C:\deps.

MingW shell:
cd /c/deps/
tar xvfz db-4.8.30.NC.tar.gz
cd db-4.8.30.NC/build_unix
../dist/configure --enable-mingw --enable-cxx --disable-shared --disable-replication
make



Boost
-----
DOS prompt:
downloaded boost jam 3.1.18
cd \boost-1.47.0-mgw
bjam toolset=gcc --build-type=complete stage
Download Boost http://sourceforge.net/projects/boost/files/boost/1.57.0/ and place in C:\deps.
DOS prompt: (use the Windows command line by typing cmd in the search bar)

cd C:\deps\boost_1_57_0\
bootstrap.bat mingw
b2 --build-type=complete --with-chrono --with-filesystem --with-program_options --with-system --with-thread toolset=gcc variant=release link=static threading=multi runtime-link=static stage



MiniUPnPc
---------
UPnP support is optional, make with USE_UPNP= to disable it.

MSYS shell:
cd /c/miniupnpc-1.6-mgw
make -f Makefile.mingw
mkdir miniupnpc
cp *.h miniupnpc/
Download http://miniupnp.free.fr/files/download.php?file=miniupnpc-1.9.20140911.tar.gz and place in C:\deps. Extract the file. 
Rename  folder from "miniupnpc-1.9.20140911" to "miniupnpc" then from a Windows command prompt:
DOS Prompt:
cd C:\deps\miniupnpc
mingw32-make -f Makefile.mingw init upnpc-static



libpng
------
Download and extract http://prdownloads.sourceforge.net/libpng/libpng-1.6.14.tar.gz?download to your deps folder. Extract.
In msys shell run 

cd /c/deps/libpng-1.6.14
configure --disable-shared
make
cp .libs/libpng16.a .libs/libpng.a



qrencode
--------
Download and extract http://fukuchi.org/works/qrencode/qrencode-3.4.4.tar.gz to your deps folder.
In msys shell run 

cd /c/deps/qrencode-3.4.4
LIBS="../libpng-1.6.14/.libs/libpng.a ../../mingw32/i686-w64-mingw32/lib/libz.a" \
png_CFLAGS="-I../libpng-1.6.14" \
png_LIBS="-L../libpng-1.6.14/.libs" \
configure --enable-static --disable-shared --without-tools

make


Download and Compile QT.
------------------------
Download and uncompress http://download.qt-project.org/official_releases/qt/4.8/4.8.6/qt-everywhere-opensource-src-4.8.6.zip to C:\Qt\4.8.6. Once again check it resides in C:\Qt\4.8.6 not C:\Qt\4.8.6\4.8.6. Due to a bug in 4.8.6 you will need to apply the patch available here https://codereview.qt-project.org/#/c/84567/3/tools/configure/configureapp.cpp. For those who can't find or work it out, you need to change the following lines in C:\Qt\4.8.6\tools\configure\configureapp.cpp or download the patched file here https://www.mediafire.com/?y4urdzfp0k0j0gm and replace it in C:\Qt\4.8.6\tools\configure\configureapp.cpp

Code: configure.cpp
2180  |  -  const QString mingwPath = dictionary["QMAKESPEC"].endsWith("-g++") ?
2180  |  +  const QString mingwPath = dictionary["QMAKESPEC"].contains("-g++") ?
2252  |   -  if (dictionary["QMAKESPEC"].endsWith("-g++")+
2252  |  +  if (dictionary["QMAKESPEC"].contains("-g++")+

From your Windows command prompt run 


cd C:\Qt\4.8.6
configure -release -opensource -confirm-license -static -no-sql-sqlite -no-qt3support -no-opengl -qt-zlib -no-gif -qt-libpng -qt-libmng -no-libtiff -qt-libjpeg -no-dsp -no-vcproj -no-openssl -no-dbus -no-phonon -no-phonon-backend -no-multimedia -no-audio-backend -no-webkit -no-script -no-scripttools -no-declarative -no-declarative-debug -qt-style-windows -qt-style-windowsxp -qt-style-windowsvista -no-style-plastique -no-style-cleanlooks -no-style-motif -no-style-cde -nomake demos -nomake examples
mingw32-makeo

Compile the Windows QT.
Edit the memecoin-qt.pro file and add the following code above OBJECTS_DIR = build

CONFIG += thread
CONFIG += static
BOOST_LIB_SUFFIX=-mgw49-mt-s-1_55
BOOST_INCLUDE_PATH=C:/deps/boost_1_55_0
BOOST_LIB_PATH=C:/deps/boost_1_55_0/stage/lib
BDB_INCLUDE_PATH=C:/deps/db-4.8.30.NC/build_unix
BDB_LIB_PATH=C:/deps/db-4.8.30.NC/build_unix
OPENSSL_INCLUDE_PATH=C:/deps/openssl-1.0.1m/include
OPENSSL_LIB_PATH=C:/deps/openssl-1.0.1m
MINIUPNPC_INCLUDE_PATH=C:/deps/
MINIUPNPC_LIB_PATH=C:/deps/miniupnpc
QRENCODE_INCLUDE_PATH=C:/deps/qrencode-3.4.4
QRENCODE_LIB_PATH=C:/deps/qrencode-3.4.4/.libs

In cmd run
set PATH=%PATH%;C:\Qt\4.8.6\bin
cd C:\memecoin\
qmake "USE_QRCODE=1" "USE_UPNP=1" "USE_IPV6=1" memecoin-qt.pro
mingw32-make -f Makefile.Release

Your Memecoin.exe will be in the release folder of the source code.

Compiling Memecoind
-------------------

In the Msys shell, you can now compile memecoind.

Code:
cd /c/memecoin/src
make -f makefile.mingw
strip memecoind.exe

Litecoin
-------
DOS prompt:
cd \litecoin\src
mingw32-make -f makefile.mingw
strip litecoind.exe
  19  src/makefile.mingw 
@@ -2,16 +2,29 @@
# Distributed under the MIT/X11 software license, see the accompanying
# file license.txt or http://www.opensource.org/licenses/mit-license.php.

USE_UPNP:=-
BOOST_SUFFIX?=-mgw46-mt-sd-1_53
USE_UPNP:=1
USE_IPV6:=1

DEPSDIR?=/usr/local
BOOST_SUFFIX?=-mgw49-mt-s-1_55

INCLUDEPATHS= \
 -I"$(CURDIR)" \

 -I"/c/deps/boost_1_55_0" \
 -I"/c/deps" \
 -I"/c/deps/db-4.8.30.NC/build_unix" \
 -I"/c/deps/openssl-1.0.1m/include"

LIBPATHS= \
 -L"$(CURDIR)/leveldb" \
 -L"/c/deps/boost_1_55_0/stage/lib" \
 -L"/c/deps/miniupnpc" \
 -L"/c/deps/db-4.8.30.NC/build_unix" \
 -L"/c/deps/openssl-1.0.1m"

LIBS= \
 -l leveldb \
 -l memenv \
 -l boost_system$(BOOST_SUFFIX) \
 -l boost_filesystem$(BOOST_SUFFIX) \
 -l boost_program_options$(BOOST_SUFFIX) \
  6  src/qt/forms/aboutdialog.ui 
@@ -95,9 +95,9 @@
       </property>
       <property name="text">
        <string>
          Copyright ?? 2009-2012 Bitcoin Developers
          Copyright ?? 2011-2012 Litecoin Developers
		  Copyright ?? 2011-2013 MemeCoin Developers
          Copyright ?? 2009-2015 Bitcoin Developers
          Copyright ?? 2011-2015 Litecoin Developers
		      Copyright ?? 2013-2015 MemeCoin Developers

          This is experimental software.
