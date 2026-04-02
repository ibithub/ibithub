// Copyright (c) 2009-2010 Satoshi Nakamoto
// Copyright (c) 2009-2018 The Bitcoin Core developers
// Copyright (c) 2018-2026 iBitHub Developers

Distributed under the MIT/X11 software license, see the accompanying
file COPYING or http://www.opensource.org/licenses/mit-license.php.

This product includes software developed by the OpenSSL Project for use in the [OpenSSL Toolkit](http://www.openssl.org/).  
This product includes cryptographic software written by Eric Young ([eay@cryptsoft.com](mailto:eay@cryptsoft.com)), and UPnP software written by Thomas Bernard.

# UNIX BUILD NOTES


## Building iBitHub 🛠️

**iBitHub** is a Litecoin 0.8-era Scrypt fork.  
Wallet and node compatibility **requires** Berkeley DB **4.8** and OpenSSL **1.0.2u**.

> ⚠️ **Do not** use newer versions of OpenSSL or Berkeley DB — they will break wallet compatibility or fail to build.

---

### Option 1 — Native Build on Ubuntu 18.04 LTS (Recommended)

**1️⃣ Update system**

```bash
sudo apt update
```

**2️⃣ Install dependencies**

```bash
sudo apt install -y build-essential git libboost-all-dev libminiupnpc-dev libevent-dev
```

```bash
sudo apt install -y automake autotools-dev pkg-config
```

**3️⃣ Build Berkeley DB 4.8 from source**

```bash
cd ~
```

```bash
wget http://download.oracle.com/berkeley-db/db-4.8.30.NC.tar.gz
```

```bash
tar -xzf db-4.8.30.NC.tar.gz
```

```bash
cd db-4.8.30.NC/build_unix
```

```bash
../dist/configure --enable-cxx --disable-shared --with-pic
```

```bash
make -j$(nproc)
```

```bash
sudo make install
```

> Installs to `/usr/local/BerkeleyDB.4.8`

**4️⃣ Build OpenSSL 1.0.2u from source**

```bash
cd ~
```

```bash
wget https://www.openssl.org/source/old/1.0.2/openssl-1.0.2u.tar.gz
```

```bash
tar -xzf openssl-1.0.2u.tar.gz
```

```bash
cd openssl-1.0.2u
```

```bash
./config --prefix=/usr/local/openssl-1.0.2 no-shared no-threads -fPIC
```

```bash
make -j$(nproc)
```

```bash
sudo make install
```

> Installs to `/usr/local/openssl-1.0.2`

**5️⃣ Clone the repository**

```bash
cd ~
```

```bash
git clone https://github.com/ibithub/ibithub.git
```

```bash
cd ibithub/src
```

**6️⃣ Update `makefile.unix` — add near the top**

```make
BDB_PREFIX = /usr/local/BerkeleyDB.4.8
OPENSSL_PREFIX = /usr/local/openssl-1.0.2

CXXFLAGS += -I$(BDB_PREFIX)/include -I$(OPENSSL_PREFIX)/include \
            -Wno-deprecated-declarations -Wno-error
LDFLAGS  += -L$(BDB_PREFIX)/lib -L$(OPENSSL_PREFIX)/lib
LIBS     += -ldb_cxx -lssl -lcrypto \
            -lboost_system -lboost_filesystem -lboost_program_options -lboost_thread
```

**7️⃣ Add CLI target in `makefile.unix`**

Locate the `ibithubd` target in `makefile.unix`:

```make
ibithubd: $(OBJS:obj/%)
	$(LINK) $(xCXXFLAGS) -o $@ $^ $(xLDFLAGS) $(LIBS)
```

Directly **above** it, add the `ibithub-cli` target:

```make
ibithub-cli: $(OBJS:obj/%)
	$(LINK) $(xCXXFLAGS) -o $@ $^ $(xLDFLAGS) $(LIBS)
```

**Explanation:**

- `ibithub-cli:` defines the CLI executable target.
- `$(OBJS:obj/%)` expands to all object files, just like `ibithubd`.
- The linking command is identical to `ibithubd` to use the same flags and libraries.

After this edit, running:

```bash
make -f makefile.unix -j$(nproc)
```

will build **both** `ibithub-cli` and `ibithubd` in your `src/` folder.

**8️⃣ Build binaries**

```bash
make -f makefile.unix clean
```

```bash
make -f makefile.unix -j$(nproc)
```

> Produces `ibithubd` and `ibithub-cli`

**9️⃣ Run iBitHub**

```bash
export LD_LIBRARY_PATH=/usr/local/openssl-1.0.2/lib:$LD_LIBRARY_PATH
```

```bash
./ibithubd -daemon
```

**🔟 Create minimal `~/.ibithub/ibithub.conf`**

```bash
nano ~/.ibithub/ibithub.conf
```

```conf
rpcuser=youruser
rpcpassword=yourpass
```

---

### Option 2 — Containerized Deterministic Build (Modern Ubuntu)

Modern Ubuntu versions ship OpenSSL 3.x → use an Ubuntu 18.04 Docker container.

**1️⃣ Install Docker on host**

```bash
sudo apt update
```

```bash
sudo apt install -y docker.io
```

```bash
sudo systemctl enable --now docker
```

```bash
sudo usermod -aG docker $USER
```

```bash
newgrp docker
```

**2️⃣ Start Ubuntu 18.04 container**

```bash
docker run -it --name ibithub-build ubuntu:18.04 /bin/bash
```

**3️⃣ Inside the container — update & install dependencies**

```bash
apt update
```

```bash
apt install -y build-essential git libboost-all-dev libminiupnpc-dev libevent-dev
```

```bash
apt install -y wget automake autotools-dev pkg-config
```

Then repeat **steps 3️⃣–8️⃣ from Option 1** inside the container, each step split as above.

**4️⃣ Copy binaries to host**

```bash
docker cp ibithub-build:/root/ibithub/src/ibithubd .
```

```bash
docker cp ibithub-build:/root/ibithub/src/ibithub-cli .
```

---

### Important Notes

- Always use **Berkeley DB 4.8** and **OpenSSL 1.0.2u**  
- Set `export LD_LIBRARY_PATH=/usr/local/openssl-1.0.2/lib:$LD_LIBRARY_PATH` before running `ibithubd` or `ibithub-cli`  
- Docker/container builds are deterministic — recommended on Ubuntu 20.04+ / 22.04+ / 24.04+ / 25.10  
- Instructions updated **March 2026** — test on fresh VMs/containers  

> Pro tip: Every full node helps secure the Scrypt network and supports play-to-earn gaming!  

**Happy building! ⭐**  
Star the repo if this helps → [github.com/ibithub/ibithub](https://github.com/ibithub/ibithub)
