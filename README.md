STM32F429i-Discovery Examples
==========================

These are the STM32F429i-Discovery board examples ported to a more current
version of libopencm3. By maintaining them this way they can be improved and 
updated without requiring the entire libopencm3-examples repo to be updated.

## Individual Applications


## Notes on porting

There is a lot of common code that was replicated in each of the example directories
and that has been collected into a single "util" directory so that it is easier to
maintain.

Removed the LDSCRIPT and replaced it with `DEVICE = STM32F429ZI`

Ideally there would be a way in the libopencm3 top library to just 'make family' 
rather than to make all families. Then in the top level of the apps directory
could be a makefile that identified each application to be built. So you could
go into one of these nested repos and type 'make' at the top and be all good.

moved console.[ch], sdram.[ch],  and clock.[ch] into util from the sdram directory.

`timer_reset()` used to be a thing, need to find out what it did and what happened
to it.

`usart_irq_console` had the old buggy ^C code in it, I removed it to prevent
tainting unfamiliar eyes with the stuff that it was doing there.

`usb_cdcacm` had a callback that returned an int rather than a `usbd_device_response`.

The DAC examples have issues that haven't been addressed yet.

`lcd-dma` failure has been replicated, but `dac-dma` also fails, so it might be in 
the DMA subsystem rather than clocks.

Noted the problems with clocks, fixed up rcc.c, pwr.c, and headers and generated PR

