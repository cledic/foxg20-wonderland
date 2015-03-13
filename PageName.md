# Compilare il Kernel per il driver spidev #

Seguire questa procedura per abilitare il driver spidev nel kernel

  * Nella directory in cui avete scompattato il kernel, eseguire:
```
make menuconfig
```

  * Nel menu che apparirà selezionate:
```
Device Drivers  --->
 [*] SPI support  --->
   <*>   Atmel SPI Controller
   <*>   User mode SPI device driver support
```

  * Fate exit e salvate.

  * Aprite il file:
```
<dir_kernel>/drivers/spi/spidev.c
```

  * Io in questo file ho aumentato la dimensione del buffer portandolo a 256KB:
```
embedded@embedded-desktop:~/foxg20/work_linux-2.6.38$ diff drivers/spi/spidev.c ../work2_linux-2.6.38/drivers/spi/spidev.c
90c90
< static unsigned bufsiz = 256 * 1024;
---
> static unsigned bufsiz = 4096;
```

  * Ricompilate eseguendo:
```
make
```