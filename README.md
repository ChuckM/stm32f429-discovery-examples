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

I have confirmed that with version 0xd0ac7fe of the library the lcd-dma demo works
that was checked in in November of 2015.

* Now testing 0x8225089 (Jan 2016) ... that still works.
* Now testing 0x781e4d9 (Aug 15, 2016) ...  that still works
* Now testing 0x755ce40 (Jan 10, 2017) ...  that DOES NOT work.
* Now testing 0x74b228f (Aug 18, 2016) ...  that DOES NOT work.
* Now testing 0xcf7d0a0 (Aug 15, 2016) one after the one above ... that works.
* Now testing 0x2db3d29 (Aug 16, 2016) mid page ... BROKEN
* Now testing 0x32a91e5 (Aug 15, 2016) down 3 commits ... still BROKEN
* Now testing 0x2614577 (Aug 15, 2016) add missing 4x9 defines ... still BROKEN <<<**THIS ONE**
* Now testing 0x543ac0f (Aug 15, 2016) between the above and cf7d0a0 ... and that one works.

So the Oldest commit where the library works is 0x543ac0f, the next newer commit 0x2614577 it
no longer works. There are many flag changes in RCC in that commit so that is where we'll look
next.

zyp figured out from that commit (that was 110 additional `#defines` and 10 deletions) one
of which changed a mask, to two variables the mask and the shift. The division constant
was not correctly being shifted so it screwed up the timing.

```
-	RCC_DCKCFGR |= RCC_DCKCFGR_PLLSAIDIVR_DIVR_8;
+	RCC_DCKCFGR |= (RCC_DCKCFGR_PLLSAIDIVR_DIVR_8 << RCC_DCKCFGR_PLLSAIDIVR_SHIFT);
```
