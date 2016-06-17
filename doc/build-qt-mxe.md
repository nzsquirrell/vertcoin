Building Vertcoin-Qt and vertcoind for Windows under mxe.cc

See http://mxe.cc

Assumptions:

MXE is built under /home/mxe/mxe (adjust instructions below if different)
Targets are i686-w64-mingw32.static and x86_64-w64-mingw32.static
Prerequisite libraries are built with mxe:

	make MXE_TARGETS='x86_64-w64-mingw32.static i686-w64-mingw32.static' gcc
	make MXE_TARGETS='x86_64-w64-mingw32.static i686-w64-mingw32.static' curl boost protobuf
	make MXE_TARGETS='x86_64-w64-mingw32.static i686-w64-mingw32.static' jpeg libmng tiff lcms lcms1 pthreads openssl postgresql freetds zlib libpng sqlite dbus miniupnpc pcre
	make MXE_TARGETS='x86_64-w64-mingw32.static i686-w64-mingw32.static' qt


Vertcoin Build instructions:

Clone Vertcoin source tree and enter base directory.


32-bit:

	make -f Makefile.Release distclean
	cd src
	make -f makefile.linux-i686-w64-mingw32.static clean
	cd secp256k1
	make distclean
	./configure --host=i686-w64-mingw32.static --prefix=/home/mxe/mxe/usr/i686-w64-mingw32.static --enable-static --disable-shared
	make
	make install
	cd ..
	make -f makefile.linux-i686-w64-mingw32.static
	i686-w64-mingw32.static-strip vertcoind.exe
	cd ..
	i686-w64-mingw32.static-qmake-qt4 "USE_QRCODE=1" "USE_UPNP=1" "USE_IPV6=1" vertcoin-qt-mingw32.static.pro
	make -f Makefile.Release


64-bit:

	make -f Makefile.Release distclean
	cd src
	make -f makefile.linux-x86_64-w64-mingw32.static clean
	cd secp256k1
	make distclean
	./configure --host=x86_64-w64-mingw32.static --prefix=/home/mxe/mxe/usr/x86_64-w64-mingw32.static --enable-static --disable-shared
	make
	make install
	cd ..
	make -f makefile.linux-x86_64-w64-mingw32.static
	x86_64-w64-mingw32.static-strip vertcoind.exe
	cd ..
	x86_64-w64-mingw32.static-qmake-qt4 "USE_QRCODE=1" "USE_UPNP=1" "USE_IPV6=1" vertcoin-qt-mingw32.static.static.pro
	make -f Makefile.Release
