#!/usr/bin/make -f
# Makefile for LinVst #

#target: prerequisites
#	./lin-patchwin
	
	
#.PHONY do_script:



#prerequisites: do_script 


    
CXX     = g++
WINECXX = wineg++

CXX_FLAGS =

PREFIX  = /usr

BIN_DIR    = $(DESTDIR)$(PREFIX)/bin
VST_DIR = ./vst

BUILD_FLAGS  = -std=c++14 -fPIC -O2 -DLVRT -DEMBED -DEMBEDDRAG -DTRACKTIONWM -DVESTIGE -DDOUBLEP -DNEWTIME -DINOUTMEM -DCHUNKBUF -DXECLOSE -DEMBEDRESIZE $(CXX_FLAGS)
# add -DNOFOCUS to the above line for alternative keyboard/mouse focus operation, add -DEMBEDRESIZE to the above line for window resizing
BUILD_FLAGS_WIN = -std=c++14 -m64 -O2 -DEMBED -DEMBEDDRAG -DWAVES2 -DTRACKTIONWM -DVESTIGE -DDOUBLEP -DNEWTIME -DINOUTMEM -DCHUNKBUF -DXECLOSE -DEMBEDRESIZE -I/usr/include/wine-development/windows -I/usr/include/wine-development/wine/windows -I/usr/include/wine/wine/windows -I../ -DRELEASE=1  -D__forceinline=inline -DNOMINMAX=1 -DUNICODE_OFF -Dstricmp=strcasecmp -Dstrnicmp=strncasecmp -DSMTG_RENAME_ASSERT=1 -fpermissive
# add -DEMBEDRESIZE to the above line for window resizing

#-DUNICODE_OFF
LINK_FLAGS   = $(LDFLAGS)

LINK_PLUGIN = -shared -lpthread -ldl -lX11 -lrt $(LINK_FLAGS)
LINK_WINE   = ../build/lib/Release/libbase.a ../build/lib/Release/libpluginterfaces.a ../build/lib/Release/libsdk.a -lole32 -L/opt/wine-stable/lib64/wine -L/opt/wine-devel/lib64/wine -L/opt/wine-staging/lib64/wine -L/usr/lib/x86_64-linux-gnu/wine-development -lpthread -lrt $(LINK_FLAGS)

TARGETS     = do_script do_script2 linvst3x.so lin-vst3-server-x.exe

# --------------------------------------------------------------

all: $(TARGETS)

do_script: 
	$(shell chmod +x ./lin-patchwin)

do_script2: 
	./lin-patchwin

linvst3x.so: linvst.unix.o remotevstclient.unix.o remotepluginclient.unix.o paths.unix.o
	$(CXX) $^ $(LINK_PLUGIN) -o $@
	
lin-vst3-server-x.exe: lin-vst-server.wine.o remotepluginserver.wine.o paths.wine.o
	$(WINECXX) $^ $(LINK_WINE) -o $@

# --------------------------------------------------------------

linvst.unix.o: linvst.cpp
	$(CXX) $(BUILD_FLAGS) -c $^ -o $@
	
remotevstclient.unix.o: remotevstclient.cpp
	$(CXX) $(BUILD_FLAGS) -c $^ -o $@
	
remotepluginclient.unix.o: remotepluginclient.cpp
	$(CXX) $(BUILD_FLAGS) -c $^ -o $@

paths.unix.o: paths.cpp
	$(CXX) $(BUILD_FLAGS) -c $^ -o $@


# --------------------------------------------------------------

lin-vst-server.wine.o: lin-vst-server.cpp
	$(WINECXX) $(BUILD_FLAGS_WIN) -c $^ -o $@

remotepluginserver.wine.o: remotepluginserver.cpp
	$(WINECXX) $(BUILD_FLAGS_WIN) -c $^ -o $@

paths.wine.o: paths.cpp
	$(WINECXX) $(BUILD_FLAGS_WIN) -c $^ -o $@


clean:
	rm -fR *.o *.exe *.so vst $(TARGETS)

install:
	install -d $(BIN_DIR)
	install -d $(VST_DIR)
	install -m 755 linvst3x.so $(VST_DIR)
	install -m 755 lin-vst3-server-x.exe lin-vst3-server-x.exe.so $(BIN_DIR)
