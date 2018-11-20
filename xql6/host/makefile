CC = gcc
PG_SERVER_INCS += -I $(shell pg_config --includedir) -I $(shell pg_config --includedir-server)
PG_SERVER_PKGLIBDIR = $(shell pg_config --pkglibdir)
CFLAGS += -O3 -fpic $(PG_SERVER_INCS)  -I${XILINX_XRT}/include

LDFLAGS = -O3 -shared -L${XILINX_XRT}/lib -L${XILINX_SDX}/lib/lnx64.o  -Wl,-rpath,${XILINX_SDX}/lib/lnx64.o -lrt -lm -ldl -lxilinxopencl -lz

TARGET = xql6.so
SOURCES1 = xql6.c q6_swemu.c fpga_q6.c
HEADERS = xql6.h q6_swemu.h
OBJECTS1 = $(SOURCES1:.c=.o)

all: $(TARGET)

$(TARGET): check-env $(OBJECTS1)
	$(CC) $(OBJECTS1) -o $(TARGET) $(LDFLAGS) 

clean:
	$(RM) $(OBJECTS1) $(TARGET)

check-env:
ifndef XILINX_SDX
	$(error XILINX_SDX is not defined)
endif
ifndef XILINX_XRT
	$(error XILINX_XRT is not defined)
endif

# sudo make install
install: $(TARGET)
	cp $(TARGET) $(PG_SERVER_PKGLIBDIR)

