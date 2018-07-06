OBJS = sdram.o clock.o console.o lcd-spi.o

BINARY = lcd-dma
CSTD = -std=gnu99

# we use sin/cos from the library
LDLIBS += -lm

DEVICE = STM32F429ZI

include ../../Makefile.include
