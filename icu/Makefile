VER=55_1

all: icu-$(VER).pkg

out/usr/local/bin/icu-config: icu4c-$(VER)-src.tgz
	mkdir -p out
	tar -xzf icu4c-$(VER)-src.tgz
	cd icu/source && ./configure --prefix=/usr/local && gnumake && gnumake install DESTDIR=$(CURDIR)/out


icu-$(VER).pkg: icu4c-$(VER)-src.tgz out/usr/local/bin/icu-config 
	productbuild  --content out/ icu-$(VER).pkg

icu4c-$(VER)-src.tgz:
	curl -o $@ http://download.icu-project.org/files/icu4c/55.1/icu4c-$(VER)-src.tgz

clean:
	rm -rf out icu
distclean: clean
	rm -f icu-$(VER).pkg icu4c-$(VER)-src.tgz
