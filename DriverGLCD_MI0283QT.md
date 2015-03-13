# Questo è un semplice programma per pilotare il display MI0283QT #

Il programma si basa su di un contest che ho seguito per la mBed [1](1.md).

Prima di tutto bisogna compilare il kernel con il driver spidev.
Modificare il file board-foxg20.c come segue, abilitare il driver nel menuconfig e compilare.

```
user@ubuntu:~/foxg20/linux-2.6.38/arch/arm/mach-at91$ diff board-foxg20.c board-foxg20.c.ORG 
138,143d137
<         {       /* SPI1 test, PB0,1,2  and CS0 (PB3) */
<                 .modalias       = "spidev",
<                 .chip_select    = 0,
<                 .max_speed_hz   = 40 * 1000 * 1000,
<                 .bus_num        = 1,
<         }, 
```

Il programma ha una serie di opzioni con cui eseguirlo:
```
./MI0283QT_lcd: option requires an argument -- 'h'
Usage: ./MI0283QT_lcd [-Dnpfcxywhrgb]
  -D --device <val>             device to use (default /dev/spidev1.1)
  -f --file <name>      file RGB to draw
  -n --noreset                  no reset is performed
  -c --clear                    redraw the screeen with color if supplied
                                or WHITE. Execute first if with -f opt
  -x --xpos <val>       x position to start to draw
  -y --ypos <val>       y position to start to draw
  -w --width <val>      width to draw
  -h --height <val>             height to draw
  -r --red <val>                red color
  -g --green <val>              green color
  -b --blue <val>               blue color
  Reset pin default on: PB18,J7.3,ID 82
```

In pratica lo si esegue una prima volta senza l'opzione "-n" e poi le successive inserendola per evitare un nuovo reset dell'LCD.

In questa versione, molto "quick-and-dirty", il pin di reset è fisso, come dice lo "usage".



[1](1.md) http://circuitcellar.com/contests/nxpmbeddesignchallenge/winners/DE3803.htm