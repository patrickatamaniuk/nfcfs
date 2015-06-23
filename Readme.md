Passthrough filesystem for OS X
===============================

which normalizes directory listings to unicode normal form C (NFC).
Useful for subversion and other tools in context with Umlauts and other unicode issues on mixed os environments

2015 Patrick Atamaniuk

Prerequisites
-------------

Install a current osxfuse (2.7.5 currently)
You get it from https://osxfuse.github.io/

Building
--------

1. Create the icu libraries (unicode libraries):

```
cd icu && make
```

2. Install the resulting pkg file

3. Compile the fuse filesystem

```
cd fuse && make
```

Install
-------

```
sudo cp fuse/nfcfs /usr/local/bin
```

Usage
-----

```
$ sudo ./nfcfs _mountpoint_ -omodules=threadid:subdir,subdir=${HOME}/some_directory -oallow_other,native_xattr,volname=nfcFS
```
