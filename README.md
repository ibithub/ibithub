# iBitHub (IBH) Green Lightning Playable

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

## Network Status (November 19, 2025)
| Specification                | Value                              |
|------------------------------|------------------------------------|
| **Block Height**             | 657,676                            |
| **Circulating Supply**       | 18,547,919 IBH (88.32%)            |
| **Remaining**                | ~2,452,081 IBH                     |
| **Daily Issuance**           | 1,728 IBH                          |
| **Next Halving**             | ~July 2027 (block 840,000)         |

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

## Building iBitHub
```bash
git clone https://github.com/ibithub/ibithub.git
cd ibithub/src

make -f makefile.unix
./ibithubd

Build requirements

sudo apt-get install build-essential
sudo apt-get install libssl-dev

Ubuntu 12.04

sudo apt-get install libboost-all-dev

Other Ubuntu / Debian

sudo apt-get install libdb4.8-dev libdb4.8++-dev
sudo apt-get install libboost1.37-dev

(If using Boost 1.37, append -mt to the boost libraries in the makefile)Optional (UPnP)bash

sudo apt-get install libminiupnpc-dev

Warning: Use Berkeley DB 4.8 only — newer versions break wallet compatibility.
Last updated: November 19, 2025

