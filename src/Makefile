#
# Copyright (c) 2026 Yuichi Nakamura (@yunkya2)
#
# The MIT License (MIT)
#
# Permission is hereby granted, free of charge, to any person obtaining a copy
# of this software and associated documentation files (the "Software"), to deal
# in the Software without restriction, including without limitation the rights
# to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
# copies of the Software, and to permit persons to whom the Software is
# furnished to do so, subject to the following conditions:
#
# The above copyright notice and this permission notice shall be included in
# all copies or substantial portions of the Software.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
# AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
# OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
# THE SOFTWARE.

CROSS = m68k-xelf-
CC = $(CROSS)gcc
AS = $(CROSS)gcc
LD = $(CROSS)gcc
AR = $(CROSS)ar
RANLIB = $(CROSS)ranlib

LIBDIR = ../lib
INCLUDEDIR = ../include
BINDIR = ../bin

CFLAGS = -Os -g $(INC) $(DEFS) -MMD -MP
ASFLAGS = $(INC) $(DEFS) -MMD -MP
BUILDDIR = build

INC += -I. -I$(INCLUDEDIR)

OBJS = tsr_finddev.o tsr_keepdev.o tsr_getresidentend.o \
       tsr_setdevdata.o tsr_getdevdata.o tsr_freedev.o \
       tsr_findproc.o tsr_keepproc.o tsr_freeproc.o \
       tsr_setprocdata.o tsr_getprocdata.o \
       tsr_bpb2dpb.o tsr_attachdrive.o tsr_getlogicaldrive.o \
       tsr_detachdrive.o tsr_formatdrive.o \
       tsr_parseinitparam.o tsr_heapfree.o \
       tsr_setvectors.o tsr_checkvectors.o tsr_restorevectors.o tsr_getoldvector.o
OBJS := $(addprefix $(BUILDDIR)/,$(OBJS))

TARGET = $(LIBDIR)/libx68ktsr.a

all: $(TARGET)

$(TARGET): $(OBJS) | $(LIBDIR)
	$(AR) rcs $@ $^
	$(RANLIB) $@

$(BUILDDIR) $(LIBDIR):
	mkdir -p $@

$(BUILDDIR)/%.o: %.c | $(BUILDDIR)
	$(CC) $(CFLAGS) -c -o $@ $<

$(BUILDDIR)/%.o: %.S | $(BUILDDIR)
	$(AS) $(ASFLAGS) -c -o $@ $<

clean:
	-rm -rf $(BUILDDIR) $(LIBDIR)

-include $(BUILDDIR)/*.d

.PHONY: all clean
