Normalizing filesystem for OS X
===============================

which normalizes directory listings to unicode normal form C (NFC) based on OSXFUSE.
Useful for subversion and other tools in context with Umlauts and other unicode issues on mixed OS environments.

The Problem
-----------

Any decent OS does not really care about unicode equivalence in their filesystem. They store what they get, and most get
the filenames in NFC normalized form.

All supported filesystems on Mac OS X will normalize the paths. The path will be normalized into NFD and it will be given back that way when listing the filesystem.

This is a problem with subversion and other versioning or file replication applications in mixed OS environments.
The subversion folks are aware of that: http://wiki.apache.org/subversion/NonNormalizingUnicodeCompositionAwareness
but there is no visible progress towards a decent workaround.
Note: my tests on Mac OS X 10.9 show that not only HFS+ is affected, but also fat (MS-DOS) and nfs mounts on OS X.

This fuse filesystems mitigates the problem by mounting a path from within a HFS+ partition and exporting
it as a Volume to a dedicated location.
Now just point your favourite svn client (such as the commandline client) to that exported Volume and
check out and work as if you had a decent, sane and working filesystem.

2015 Patrick Atamaniuk

Status of the code
------------------

This is quite experimental and not well tested yet. Feedback is very welcome.
The current tested version is 0.9, the latest code will be 0.9.1

Prerequisites
-------------

Install a current osxfuse (2.7.5 currently)
You get it from https://osxfuse.github.io/

To compile the filesystem and prerequisites, you need the xtools commandline tools.
Install them with ```xcode-select --install```

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
cd fuse && sudo make install
```

Usage
-----

```
$ sudo nfcfs /tmp/sourcedir /Volumes/loop_mountpoint -oallow_other,native_xattr,volname=LoopbackFS
```

in fstab:

as root `mkdir /mnt/mountpoint`

/etc/fstab
```
/tmp/sourcedir	/mnt/mountpoint	nfcfs	allow_other,native_xattr,volname=01Repos	0	0
```

Issues
------

Of course this fixes only the filesystem layer. When operating directly on a repository using an svn client, the user must take care not to enter NFD normalized data when renaming or tagging/branching objects. First tests show that typing Umlauts won't be a problem, the input is NFC normalized. But copy-pasting from the console listing will be a problem. The user should be aware of that (unless the client software mitigates that. But there is none afaik).

Other Implementations
---------------------

After writing this i found a quite similar approach written by Thomas Andersson. Check it out here: http://nfcfs.sourceforge.net/
It does not require the icu libraries but it is based on MacFUSE which is no longer maintained.
