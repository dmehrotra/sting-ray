# Makefile
# This file holds the make rules for the libyasn

DEBUG :=

CXX  := g++ -Wall
AR  := ar
DEFS :=
INCLUDES := -I../.. -I../.. -I.
CFLAGS := -g -O2 -Wno-overloaded-virtual  -fno-exceptions -fPIC -DHAVE_GCC_FORMAT_CHECK -DHAVE_BLOCK_RETURN 
LDFLAGS:=  -L../.. -lyate
SONAME_OPT := -dynamiclib -Wl,-dylib_install_name,
YATELIBS := -L../.. -lyate 
INCFILES := ../../yateclass.h ./yateasn.h

PROGS=
LIBS = libyasn.a
OBJS = asn.o
LIBD_DEV:= libyateasn.so
LIBD_VER:= $(LIBD_DEV).5.5.1
LIBD:= ../../$(LIBD_VER) ../../$(LIBD_DEV)

LOCALFLAGS =
LOCALLIBS =
COMPILE = $(CXX) $(DEFS) $(DEBUG) $(INCLUDES) $(CFLAGS)
LINK = $(CXX) $(LDFLAGS)

prefix = /usr/local
exec_prefix = ${prefix}

# include optional local make rules
-include YateLocal.mak

.PHONY: all debug ddebug xdebug
all: $(LIBS) $(LIBD) $(PROGS)

debug:
	$(MAKE) all DEBUG=-g3 MODSTRIP=

ddebug:
	$(MAKE) all DEBUG='-g3 -DDEBUG' MODSTRIP=

xdebug:
	$(MAKE) all DEBUG='-g3 -DXDEBUG' MODSTRIP=

.PHONY: strip
strip: all
	strip --strip-debug --discard-locals $(PROGS)

.PHONY: clean
clean:
	@-$(RM) $(PROGS) $(LIBS) $(LIBD) $(OBJS) core 2>/dev/null

%.o: ./%.cpp $(INCFILES)
	$(COMPILE) -c $<

Makefile: ./Makefile.in ../../config.status
	cd ../.. && ./config.status

../../$(LIBD_VER): $(OBJS)
	$(LINK) -o $@ $(SONAME_OPT)$(LIBD_VER) $^ $(YATELIBS)

../../$(LIBD_DEV): ../../$(LIBD_VER)
	cd ../.. && ln -sf $(LIBD_VER) $(LIBD_DEV)

$(LIBS): $(OBJS)
	$(AR) rcs $@ $^
