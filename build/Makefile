AR=ar

UNAME := $(shell uname)

BASEDIR=../sources
INCLUDES = -I$(BASEDIR)/ -I ../../libhxcfe/sources -I ../sources/win32 -I ../../libhxcadaptor/sources

CFLAGS=-O3 $(INCLUDES)
LDFLAGS= -g -shared -lc -lm  -ldl

EXEC=libusbhxcfe.a

# For shared libraries:
# EXEC=libusbhxcfe.so
# CFLAGS += -fPIC (in per-platform overrides)

ifeq ($(UNAME), Linux)
CC=gcc
endif

ifeq ($(UNAME), Darwin)
CFLAGS += -arch i386 -mmacosx-version-min=10.5
endif

ifeq ($(findstring CYGWIN,$(shell uname)),CYGWIN)
EXEC=libusbhxcfe.dll
endif

all: $(EXEC)

libusbhxcfe.dll: usb_hxcfloppyemulator.o variablebitrate.o ftdi.o
	$(AR) r $@    $^ 
	mkdir -p ../../build
	cp $@ ../../build/

libusbhxcfe.dylib: usb_hxcfloppyemulator.o variablebitrate.o ftdi.o
	$(AR) r $@    $^ 
	mkdir -p ../../build
	cp $@ ../../build/

libusbhxcfe.a: usb_hxcfloppyemulator.o variablebitrate.o ftdi.o
	$(AR) r $@    $^ 
	mkdir -p ../../build
	cp $@ ../../build/

libusbhxcfe.so: usb_hxcfloppyemulator.o variablebitrate.o ftdi.o
	$(CC) -o $@    $^ $(LDFLAGS)
	mkdir -p ../../build
	cp $@ ../../build/

usb_hxcfloppyemulator.o: $(BASEDIR)/usb_hxcfloppyemulator.c
	$(CC) -o $@ -c $< $(CFLAGS)

variablebitrate.o: $(BASEDIR)/variablebitrate.c
	$(CC) -o $@ -c $< $(CFLAGS)

ftdi.o: $(BASEDIR)/ftdi.c
	$(CC) -o $@ -c $< $(CFLAGS)

ftdi_d2xxmode.o : ftdi.c ftdi.h ftd2xx.h WinTypes.h
	$(CC) -o $@ -c $< $(CFLAGS)

ftdi_libftdimode.o : ftdi.c ftdi.h ftd2xx.h WinTypes.h
	$(CC) -o $@ -c $< $(CFLAGS) -DFTDILIB

clean:
	rm -rf *.o

mrproper: clean
	rm -rf $(EXEC)

.PHONY: clean mrproper
