#!/usr/bin/make -f
# Makefile for LinVst #

CXX     = g++

CXX_FLAGS =

PREFIX  = /usr

BIN_DIR    = $(DESTDIR)$(PREFIX)/bin

BUILD_FLAGS  = `pkg-config --cflags gtk+-3.0` $(CXX_FLAGS)

LINK_FLAGS   = $(LDFLAGS)

LINK_APP = -no-pie `pkg-config --libs gtk+-3.0` $(LINK_FLAGS)

TARGETS     = linvst3xconvert

# --------------------------------------------------------------

all: $(TARGETS)

linvst3xconvert: linvstconvertgtk.o
	$(CXX) $^ $(LINK_APP) -o $@
	
linvstconvertgtk.o: linvstconvertgtk.cpp
	$(CXX) $(BUILD_FLAGS) -c $^ -o $@	
	
clean:
	rm -fR *.o $(TARGETS)

install:
	install -m 755 linvst3xconvert $(BIN_DIR)
