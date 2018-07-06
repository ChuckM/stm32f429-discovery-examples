OBJS = ../util/sdram.o ../util/clock.o console.o lcd-spi.o

CFLAGS += -I../../libopencm3/include
BINARY = lcd-dma
CSTD = -std=gnu99

# we use sin/cos from the library
LDLIBS += -lm

DEVICE = STM32F429ZI

include ../../Makefile.include
