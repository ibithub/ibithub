// Copyright (c) 2009-2010 Satoshi Nakamoto

// Copyright (c) 2009-2018 The Bitcoin Core developers

// Copyright (c) 2018-2026 iBitHub Developers

Distributed under the MIT/X11 software license, see the accompanying
file COPYING or http://www.opensource.org/licenses/mit-license.php.
This product includes software developed by the OpenSSL Project for use in the [OpenSSL Toolkit](http://www.openssl.org/). This product includes
cryptographic software written by Eric Young ([eay@cryptsoft.com](mailto:eay@cryptsoft.com)), and UPnP software written by Thomas Bernard.

UNIX BUILD NOTES
====================

## Building iBitHub 🛠️

**iBitHub** is a Litecoin 0.8-era Scrypt fork.  
Wallet compatibility **requires** Berkeley DB **4.8** and OpenSSL **1.0.2u**.

> ⚠️ **Do not** use newer versions of OpenSSL or Berkeley DB — they will break wallet compatibility or fail to build.

### Option 1 — Native Build on Ubuntu 18.04 LTS (Recommended for simplicity)

1. **Update system & install dependencies**

```bash
sudo apt update
sudo apt install -y build-essential git libboost-all-dev libminiupnpc-dev libevent-dev \
                    automake autotools-dev pkg-config
```
Build Berkeley DB 4.8 from sourcebash
```
cd ~
wget http://download.oracle.com/berkeley-db/db-4.8.30.NC.tar.gz
tar -xzf db-4.8.30.NC.tar.gz
cd db-4.8.30.NC/build_unix
../dist/configure --enable-cxx --disable-shared --with-pic
make -j$(nproc)
sudo make install
```
→ Installs to /usr/local/BerkeleyDB.4.8

Build OpenSSL 1.0.2u from sourcebash
```
cd ~
wget https://www.openssl.org/source/old/1.0.2/openssl-1.0.2u.tar.gz
tar -xzf openssl-1.0.2u.tar.gz
cd openssl-1.0.2u
./config --prefix=/usr/local/openssl-1.0.2 no-shared no-threads -fPIC
make -j$(nproc)
sudo make install
```
→ Installs to /usr/local/openssl-1.0.2

Clone the repositorybash
```
cd ~
git clone https://github.com/ibithub/ibithub.git
cd ibithub/src
```
Update makefile.unix — add near the top:
```
BDB_PREFIX = /usr/local/BerkeleyDB.4.8
OPENSSL_PREFIX = /usr/local/openssl-1.0.2

CXXFLAGS += -I$(BDB_PREFIX)/include -I$(OPENSSL_PREFIX)/include \
            -Wno-deprecated-declarations -Wno-error
LDFLAGS  += -L$(BDB_PREFIX)/lib -L$(OPENSSL_PREFIX)/lib
LIBS     += -ldb_cxx -lssl -lcrypto \
            -lboost_system -lboost_filesystem -lboost_program_options -lboost_thread
```
Remove any reference to alert.o if it exists.

Build
```
make -f makefile.unix clean
make -f makefile.unix -j$(nproc)
```
→ Produces ibithubd and ibithub-cli

Run iBitHub
```
export LD_LIBRARY_PATH=/usr/local/openssl-1.0.2/lib:$LD_LIBRARY_PATH
./ibithubd -daemon
```
Create a minimal ~/.ibithub/ibithub.conf:
```
rpcuser=youruser
rpcpassword=yourpass
```
Option 2 — Containerized Deterministic Build on modern Ubuntu (e.g. 25.10)

Modern Ubuntu versions ship OpenSSL 3.x and incompatible toolchains → use an Ubuntu 18.04 Docker container for a reproducible legacy environment.

Install Docker on host
```
sudo apt update
sudo apt install -y docker.io
sudo systemctl enable --now docker
sudo usermod -aG docker $USER
newgrp docker
```
Start Ubuntu 18.04 container
```
docker run -it --name ibithub-build ubuntu:18.04 /bin/bash
```
Inside the container — install dependencies & follow Option 1 stepsbash
```
apt update
apt install -y build-essential git libboost-all-dev libminiupnpc-dev libevent-dev \
               wget automake autotools-dev pkg-config
```
Then repeat steps 2–6 from Option 1 inside the container.

Copy binaries to host (from another terminal)bash
```
docker cp ibithub-build:/root/ibithub/src/ibithubd .
docker cp ibithub-build:/root/ibithub/src/ibithub-cli .
```
Important Notes:

Always use Berkeley DB 4.8 and OpenSSL 1.0.2u for compatibility.

Set export LD_LIBRARY_PATH=/usr/local/openssl-1.0.2/lib:$LD_LIBRARY_PATH every time before running ibithubd or ibithub-cli.

Docker/container builds are deterministic — highly recommended on Ubuntu 20.04+ / 22.04+ / 24.04+ / 25.10 to avoid toolchain/OpenSSL mismatches.

These instructions are updated March 2026 — test on fresh VMs/containers.

Happy building!
Star the repo if this helps →
