// Copyright (c) 2009-2010 Satoshi Nakamoto

// Copyright (c) 2009-2018 The Bitcoin Core developers

// Copyright (c) 2018-2021 iBitHub Developers

Distributed under the MIT/X11 software license, see the accompanying
file COPYING or http://www.opensource.org/licenses/mit-license.php.
This product includes software developed by the OpenSSL Project for use in the [OpenSSL Toolkit](http://www.openssl.org/). This product includes
cryptographic software written by Eric Young ([eay@cryptsoft.com](mailto:eay@cryptsoft.com)), and UPnP software written by Thomas Bernard.

UNIX BUILD NOTES
====================

To Build
---------------------

	cd src/
	make -f makefile.unix		# Headless ibithub

See readme-qt.rst for instructions on building Ibithub-Qt, the graphical user interface.

Dependencies
---------------------

 Library     Purpose           Description
 -------     -------           -----------
 libssl      SSL Support       Secure communications
 libdb4.8    Berkeley DB       Blockchain & wallet storage
 libboost    Boost             C++ Library
 miniupnpc   UPnP Support      Optional firewall-jumping support

[miniupnpc](http://miniupnp.free.fr/) may be used for UPnP port mapping.  It can be downloaded from [here](
http://miniupnp.tuxfamily.org/files/).  UPnP support is compiled in and
turned off by default.  Set USE_UPNP to a different value to control this:

	USE_UPNP=     No UPnP support miniupnp not required
	USE_UPNP=0    (the default) UPnP support turned off by default at runtime
	USE_UPNP=1    UPnP support turned on by default at runtime

IPv6 support may be disabled by setting:

	USE_IPV6=0    Disable IPv6 support

Licenses of statically linked libraries:
 Berkeley DB   New BSD license with additional requirement that linked
               software must be free open source
 Boost         MIT-like license
 miniupnpc     New (3-clause) BSD license

- Versions used in this release:
-  GCC           4.3.3
-  OpenSSL       1.0.1c
-  Berkeley DB   4.8.30.NC
-  Boost         1.37
-  miniupnpc     1.6

Dependency Build Instructions: Ubuntu & Debian
----------------------------------------------

# Compile iBitHub Core on Ubuntu 16.04 LTS

# Update & Upgrade the System
sudo apt-get update

sudo apt-get upgrade

# Install dependencies there might be more based on your system
 However below instructions are for the fresh Ubuntu install/server
 Please carefully watch the logs because if something could not be install
 You have to make sure it is installed properly by trying the command or that particular
 dependency again

sudo apt-get install build-essential libtool autotools-dev autoconf pkg-config libssl-dev

sudo apt-get install libboost-all-dev

sudo apt-get install libqt5gui5 libqt5core5a libqt5dbus5 qttools5-dev qttools5-dev-tools libprotobuf-dev protobuf-compiler

sudo apt-get install libqrencode-dev autoconf openssl libssl1.0-dev libevent-dev

sudo apt-get install libminiupnpc-dev

# Download iBitHub Source code
# ----------------------------
cd ~

git clone https://github.com/ibithub/ibithub.git

# Bitcoin uses the Berkley DB 4.8
 We need to install it as well
# Download & Install Berkley DB
# -----------------------------
cd ~

mkdir ibithub/db4/

wget 'http://download.oracle.com/berkeley-db/db-4.8.30.NC.tar.gz'

tar -xzvf db-4.8.30.NC.tar.gz

cd db-4.8.30.NC/build_unix/

../dist/configure --enable-cxx --disable-shared --with-pic --prefix=/home/theusername/ibithub/db4/

make install

cd ~

sudo apt-get install libdb++-dev

# Compile iBitHub with Berkley DB 4.8
# -----------------------------------
cd ibithub/src

make -f makefile.unix

./ibithubd

------------------------------------

Build requirements:

	sudo apt-get install build-essential
	
	sudo apt-get install libssl-dev


for Ubuntu 12.04:

	sudo apt-get install libboost-all-dev

 db4.8 packages are available [here](https://launchpad.net/~bitcoin/+archive/bitcoin).

 Ubuntu precise has packages for libdb5.1-dev and libdb5.1++-dev,
 but using these will break binary wallet compatibility, and is not recommended.

for other Ubuntu & Debian:

	sudo apt-get install libdb4.8-dev
	sudo apt-get install libdb4.8++-dev
	sudo apt-get install libboost1.37-dev
 (If using Boost 1.37, append -mt to the boost libraries in the makefile)

Optional:

	sudo apt-get install libminiupnpc-dev (see USE_UPNP compile flag)


Notes
-----
The release is built with GCC and then "strip bitcoind" to strip the debug
symbols, which reduces the executable size by about 90%.


miniupnpc
---------
	tar -xzvf miniupnpc-1.6.tar.gz
	cd miniupnpc-1.6
	make
	sudo su
	make install


Berkeley DB
-----------
You need Berkeley DB 4.8.  If you have to build Berkeley DB yourself:

	../dist/configure --enable-cxx
	make


Boost
-----
If you need to build Boost yourself:

	sudo su
	./bootstrap.sh
	./bjam install


Security
--------
To help make your ibithub installation more secure by making certain attacks impossible to
exploit even if a vulnerability is found, you can take the following measures:

* Position Independent Executable
    Build position independent code to take advantage of Address Space Layout Randomization
    offered by some kernels. An attacker who is able to cause execution of code at an arbitrary
    memory location is thwarted if he doesn't know where anything useful is located.
    The stack and heap are randomly located by default but this allows the code section to be
    randomly located as well.

    On an Amd64 processor where a library was not compiled with -fPIC, this will cause an error
    such as: "relocation R_X86_64_32 against `......' can not be used when making a shared object;"

    To build with PIE, use:
    make -f makefile.unix ... -e PIE=1

    To test that you have built PIE executable, install scanelf, part of paxutils, and use:

    	scanelf -e ./ibithub

    The output should contain:
     TYPE
    ET_DYN

* Non-executable Stack
    If the stack is executable then trivial stack based buffer overflow exploits are possible if
    vulnerable buffers are found. By default, bitcoin should be built with a non-executable stack
    but if one of the libraries it uses asks for an executable stack or someone makes a mistake
    and uses a compiler extension which requires an executable stack, it will silently build an
    executable without the non-executable stack protection.

    To verify that the stack is non-executable after compiling use:
    `scanelf -e ./ibithub`

    the output should contain:
	STK/REL/PTL
	RW- R-- RW-

    The STK RW- means that the stack is readable and writeable but not executable.


