# iBitHub (IBH) Blockchain Cryptocurrency

[![GitHub Repo stars](https://img.shields.io/github/stars/ibithub/ibithub?style=social)](https://github.com/ibithub/ibithub)

[![Price](https://img.shields.io/badge/price-%240.0062-brightgreen)](https://www.dex-trade.com/trade/IBH_USDT)

**iBitHub** — U.S.-based, fair-launched Scrypt PoW coin (2018)  
No ICO • No pre-mine • 100% mined • Merge-mined with Dogecoin  
21 million supply cap • Play-to-earn gaming • Built to last

> Source code preserved for 1,000 years in GitHub’s Arctic Code Vault

---

## Core Specifications
| Specification                | Value                              |
|------------------------------|------------------------------------|
| **Algorithm**                | Scrypt                             |
| **Consensus**                | Proof of Work (PoW)                |
| **Merge Mining**             | Yes – with Dogecoin (AuxPoW)       |
| **Total Supply**             | 21,000,000 IBH                     |
| **Block Time**               | 5 minutes                          |
| **Current Reward**           | 6.25 IBH                           |
| **Halving**                  | Every 210,000 blocks               |
| **Difficulty Retarget**      | Every block                        |
| **RPC Port**                 | 2333                               |
| **P2P Port**                 | 2332                               |

---

## Distribution & Halving Schedule (Full 34 Eras)
| Era | Block Height | Approx. Year | Reward Before → After     | Coins This Era | Cumulative Mined |
|-----|--------------|--------------|---------------------------|----------------|------------------|
| 0   | 0            | 2018         | – → 50 IBH                | 10,500,000     | 50.00%           |
| 1   | 210,000      | 2021         | 50 → 25 IBH               | 5,250,000      | 75.00%           |
| 2   | 420,000      | 2023         | 25 → 12.5 IBH             | 2,625,000      | 87.50%           |
| 3   | 630,000      | 2025         | 12.5 → 6.25 IBH           | 1,312,500      | 93.75%           |
| 4   | 840,000      | 2027         | 6.25 → 3.125 IBH          | 656,250        | 96.875%          |
| 5   | 1,050,000    | 2029         | 3.125 → 1.5625 IBH        | 328,125        | 98.4375%         |
| 6   | 1,260,000    | 2031         | 1.5625 → 0.78125 IBH      | 164,062        | 99.21875%        |
| 7   | 1,470,000    | 2033         | 0.78125 → 0.390625 IBH    | 82,031         | 99.609375%       |
| 8   | 1,680,000    | 2035         | 0.390625 → 0.1953125 IBH  | 41,015         | 99.8046875%      |
| 9   | 1,890,000    | 2037         | 0.1953125 → 0.09765625    | 20,507         | 99.90234375%     |
| …   | …            | …            | …                         | …              | …                |
| 20  | 4,200,000    | ~2075        | ~0.0000488 → ~0.0000244   | ~10            | 99.99999…%       |
| 33  | 6,930,000    | ~2138        | ~0.00000012 → ~0.00000006 | <1             | ≈100%            |
| 34  | 7,140,000    | ~2140        | Reward < ~0.00000001      | 0              | 100%             |

Total supply reaches **exactly 21,000,000 IBH** around year **2140**, just like Bitcoin.

---

**Quick Links**  
Website & Wallet → [ibithub.com](https://ibithub.com)  
Explorer → [explorer.ibithub.com:12555](http://explorer.ibithub.com:12555/)  
Play-to-Earn Poker → [poker.ibithub.com](https://poker.ibithub.com)  
Trade → [Dex-Trade IBH/USDT](https://www.dex-trade.com/trade/IBH_USDT)  
Discord → [discord.gg/KfS3FSf](https://discord.gg/KfS3FSf)  
X → [@goplayonline](https://twitter.com/goplayonline)

**Star** Star • **Mine** • **Play** • **Build**  
**iBitHub — Green. Scarce. Playable.**

---

## Building iBitHub 🛠️

**iBitHub** is a Litecoin 0.8-era Scrypt fork.  
Wallet compatibility **requires** Berkeley DB **4.8** and OpenSSL **1.0.2u**.

> ⚠️ **Do not** use newer versions of OpenSSL or Berkeley DB — they will break wallet compatibility or fail to build.

### Option 1 — Native Build on Ubuntu 18.04 LTS (Recommended for simplicity)

1. **Update system & install dependencies**

```
sudo apt update
sudo apt install -y build-essential git libboost-all-dev libminiupnpc-dev libevent-dev \
                    automake autotools-dev pkg-config
```
Build Berkeley DB 4.8 from source
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

Build OpenSSL 1.0.2u from source
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

Clone the repository
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
Inside the container — install dependencies & follow Option 1 steps
```
apt update
apt install -y build-essential git libboost-all-dev libminiupnpc-dev libevent-dev \
               wget automake autotools-dev pkg-config
```
Then repeat steps 2–6 from Option 1 inside the container.

Copy binaries to host (from another terminal)
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
