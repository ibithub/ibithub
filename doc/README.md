Ibithub Core
=============

Setup
---------------------
Ibithub Core is the original Ibithub client and it builds the backbone of the network. It downloads and, by default, stores the entire history of Ibithub transactions (which is currently more than 7 GBs); depending on the speed of your computer and network connection, the synchronization process can take anywhere from a few hours to a day or more.

To download Ibithub Core, visit [ibithub.org](https://ibithub.org).

Running
---------------------
The following are some helpful notes on how to run Ibithub on your native platform.

### Unix

Unpack the files into a directory and run:

- `bin/ibithub-qt` (GUI) or
- `bin/ibithubd` (headless)

### Windows

Unpack the files into a directory, and then run ibithub-qt.exe.

### OS X

Drag Ibithub-Core to your applications folder, and then run Ibithub-Core.

### Need Help?

* See the documentation at the [Ibithub Wiki](https://ibithub.info/)
for help and more information.
* Ask for help on [#ibithub](http://webchat.freenode.net?channels=ibithub) on Freenode. If you don't have an IRC client use [webchat here](http://webchat.freenode.net?channels=ibithub).
* Ask for help on the [IbithubTalk](https://ibithubtalk.io/) forums.

Building
---------------------
The following are developer notes on how to build Ibithub on your native platform. They are not complete guides, but include notes on the necessary libraries, compile flags, etc.

- [OS X Build Notes](build-osx.md)
- [Unix Build Notes](build-unix.md)
- [Windows Build Notes](build-windows.md)
- [OpenBSD Build Notes](build-openbsd.md)
- [Gitian Building Guide](gitian-building.md)

Development
---------------------
The Ibithub repo's [root README](/README.md) contains relevant information on the development process and automated testing.

- [Developer Notes](developer-notes.md)
- [Release Notes](release-notes.md)
- [Release Process](release-process.md)
- [Source Code Documentation (External Link)](https://dev.visucore.com/ibithub/doxygen/)
- [Translation Process](translation_process.md)
- [Translation Strings Policy](translation_strings_policy.md)
- [Travis CI](travis-ci.md)
- [Unauthenticated REST Interface](REST-interface.md)
- [Shared Libraries](shared-libraries.md)
- [BIPS](bips.md)
- [Dnsseed Policy](dnsseed-policy.md)
- [Benchmarking](benchmarking.md)

### Resources
* Discuss on the [IbithubTalk](https://ibithubtalk.io/) forums.
* Discuss general Ibithub development on #ibithub-dev on Freenode. If you don't have an IRC client use [webchat here](http://webchat.freenode.net/?channels=ibithub-dev).

### Miscellaneous
- [Assets Attribution](assets-attribution.md)
- [Files](files.md)
- [Fuzz-testing](fuzzing.md)
- [Reduce Traffic](reduce-traffic.md)
- [Tor Support](tor.md)
- [Init Scripts (systemd/upstart/openrc)](init.md)
- [ZMQ](zmq.md)

License
---------------------
Distributed under the [MIT software license](/COPYING).
This product includes software developed by the OpenSSL Project for use in the [OpenSSL Toolkit](https://www.openssl.org/). This product includes
cryptographic software written by Eric Young ([eay@cryptsoft.com](mailto:eay@cryptsoft.com)), and UPnP software written by Thomas Bernard.
