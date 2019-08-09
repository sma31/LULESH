MPI_FLAGS = -DUSE_MPI -I$(MPICH_DIR)/include

CHARMDIR = $(HOME)/charmworks/charm/netlrts-linux-x86_64-cuda

AMPICXX = $(CHARMDIR)/bin/ampicxx

NVCC		= nvcc
FLAGS		= -arch=sm_60 -I$(CHARMDIR)/include -I$(CHARMDIR)/include/ampi -DUSE_MPI -O0 -g
DFLAGS	= $(MPI_FLAGS) -lineinfo
RFLAGS 	= $(MPI_FLAGS) -O3 -DNDEBUG

#SILO_INCLUDES := /usr/local/silo-4.8/include
#SILO_LIBS := /usr/local/silo-4.8/lib

LINKFLAGS = -I/usr/local/cuda/include -L/usr/local/cuda/lib64 -lcudart
#LINKFLAGS += -L$(SILO_LIBS) -lsilo

#INC_SILO:= -I$(SILO_INCLUDES)

all: release

debug: LINKFLAGS +=

release: 	#FLAGS += $(RFLAGS)
debug: 		#FLAGS += $(DFLAGS)

release: lulesh
debug: lulesh

lulesh: allocator.o lulesh.o lulesh-comms.o lulesh-comms-gpu.o
	$(AMPICXX) $(LINKFLAGS) allocator.o lulesh.o lulesh-comms.o lulesh-comms-gpu.o -o lulesh

allocator.o: allocator.cu vector.h
	$(NVCC) $(FLAGS) allocator.cu -I ./ -c -o allocator.o

lulesh.o: lulesh.cu util.h vector.h allocator.h
	$(NVCC) $(FLAGS) lulesh.cu -I ./  $(INC_SILO) -c -o lulesh.o

lulesh-comms.o: lulesh-comms.cu
	$(NVCC) $(FLAGS) lulesh-comms.cu -I ./ -c -o lulesh-comms.o

lulesh-comms-gpu.o: lulesh-comms-gpu.cu
	$(NVCC) $(FLAGS) lulesh-comms-gpu.cu -I ./ -c -o lulesh-comms-gpu.o

clean:
	rm -rf allocator.o lulesh-comms.o lulesh-comms-gpu.o lulesh.o lulesh xyz.asc regNumList.txt
