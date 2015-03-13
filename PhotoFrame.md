# Raccolta di programmi Perl per visualizzare foto e news #

  * Il file .tar.gz contiene tutto il necessario per far lavorare i script.

  * Serve però che nella FOXG20 siano installati:
  1. wget
  1. imagemagick
  1. rdate

  * Scompattando il programma, si creerà una directory photoframe. Al suo interno altri due folder: cifre/ e images/

  * Il primo contiene i file .RGB delle cifre usate per visualizzare l'orologio. Il folder images/ invece è un folder di lavoro, in cui i script scaricano le immagini che vengono poi convertite in RGB.

  * All'interno della directory photoframe c'è anche il programma ed il sorgente in .C del driver che pilota in SPI l'LCD. Anche questo programma è in via di miglioramento.

  * Il programma da eseguire è:
```
./photoframe.pl
```