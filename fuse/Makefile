LDFLAGS=-L/usr/local/lib -licui18n -licuuc -licudata -licuio
CFLAGS=-I${HOME}/usr/local/include


TARGETS = nfcfs

# Root for OSXFUSE includes and libraries
OSXFUSE_ROOT = /usr/local
#OSXFUSE_ROOT = /opt/local

INCLUDE_DIR = $(OSXFUSE_ROOT)/include/osxfuse/fuse
LIBRARY_DIR = $(OSXFUSE_ROOT)/lib

CC ?= gcc

CFLAGS_OSXFUSE = -I$(INCLUDE_DIR) -L$(LIBRARY_DIR)
CFLAGS_OSXFUSE += -D_FILE_OFFSET_BITS=64
CFLAGS_OSXFUSE += -D_DARWIN_USE_64_BIT_INODE

CFLAGS_EXTRA = -Wall -g $(CFLAGS)

LIBS = -losxfuse

.c:
	$(CC) $(CFLAGS_OSXFUSE) $(CFLAGS_EXTRA) $(CFLAGS) -o $@ $< $(LIBS) $(LDFLAGS)

all: info

nfcfs: nfcfs.c

info: $(TARGETS)
	@echo
	@echo Compiled. The following is a typical way to run the loopback file system. In
	@echo this example, /tmp/dir is an existing directory whose contents will become
	@echo available in the existing mount point /Volumes/loop:
	@echo
	@echo "sudo ./nfcfs /Volumes/loop -omodules=threadid:subdir,subdir=/tmp/dir -oallow_other,native_xattr,volname=LoopbackFS"
	@echo

clean:
	rm -f $(TARGETS) *.o
	rm -rf *.dSYM

DYLD_LIBRARY_PATH=/usr/local/lib
testmnt:
	sudo ./nfcfs ${HOME}/.repos-export  ${HOME}/mnt/1 -oallow_other,native_xattr,volname=nfcFS
testmntdbg:
	sudo DYLD_LIBRARY_PATH=$(DYLD_LIBRARY_PATH) ./nfcfs ${HOME}/.repos-export ${HOME}/mnt/1 -d -oallow_other,native_xattr,volname=nfcFS
testumnt:
	sudo umount ${HOME}/mnt/1
