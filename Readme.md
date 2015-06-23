Passthrough filesystem for OS X
===============================

which normalizes directory listings to unicode normal form C (NFC).
Useful for subversion and other tools in context with Umlauts and other unicode issues on mixed os environments.

The Problem
-----------

Any decent os does not really care about unicode equivalence in their filesystem. They store what they get, and most get
the filenames in NFC normalized form.

All supported filesystems on Mac OS X will normalize the paths. The path will be normalized into NFD and it will be given back that way when listing the filesystem.

This is a problem with subversion and other versioning or file replication applications in mixed os environments.
The subversion folks are aware of that: http://wiki.apache.org/subversion/NonNormalizingUnicodeCompositionAwareness but there is no visible progress to a decent workaround.
Note: my tests on Mac OS X 10.9 show that not only HFS+ is affected, but also fat (MS-DOS) and nfs mounts on OS X.

This fuse filesystems mitigates the problem by mounting a path from within a HFS+ partition to a dedicated location. Now just point your favourite svn client (such as the commandline client) to that exported Volume and check out and work as if you had a decent, sane and working filesystem.

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

```
sudo installer -pkg icu/icu-55_1.pkg -target /
```

3. Compile the fuse filesystem

This requires installed osxfuse and icu

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
$ sudo nfcfs _mountpoint_ -omodules=threadid:subdir,subdir=${HOME}/some_directory -oallow_other,native_xattr,volname=nfcFS
```
