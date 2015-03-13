# Invio di immagini tramite Bluetooth #

Ho installato i seguenti package:
```
apt-get install bluez
apt-get install obexftp obexpushd
```
Il mio kernel aveva già i moduli disponibili per il bluetooth. Dovete quindi verificare se anche nella vostra implementazione sono disponibili.
I comandi che eseguo per attivarlo sono i seguenti:
```
# pubblico il servizio BT con un nome nuovo
hciconfig hci0 name photoframe
# lo rendo visibile
hciconfig hci0 piscan
# imposto la password da inserire nella fase di collegamento...
bluetooth-agent 0000 &
# metto il dispositivo in attesa di files...
# obexpushd -B -o "/root/photoframe/bt_images" -p "/var/run/obexpushd.pid"
obexpushd -B -p "/var/run/obexpushd.pid" -s ./bt_filetransfer.sh
```
  * Questo script pubblica la fox con il nome: **photoframe**. Programma la key da inserire durante il pairing come: **000**. Si mette poi in attesa della ricezione di un file.
  * Il programma **obexpushd** viene eseguito in modo che lo scambio file avvenga tramite uno script: **bt\_filetransfer.sh**. Questo script è tratto da un esempio disponibilie nel file: _/usr/share/doc/obexpushd/examples/file\_storage.sh_
  * La funzione di questo script è di accettare il transfer file e di spostare il file in una directory di lavoro.
```
#!/bin/bash

set -e

ROOT_PATH="/root/photoframe/bt_images"
IMG_PATH="/root/photoframe/images"
NEW_NAME="${IMG_PATH}/bt_$((RANDOM%30+1)).jpg"

#
# compatible with obexpushd >= 0.6
#

MODE="$1"
FROM=""
NAME=""
SUB_PATH="${ROOT_PATH}/"
LENGTH="0"
MIMETYPE=""
while read LINE; do
        echo "${LINE}" 1>&2
        if ( test -z "${LINE}" ); then
                break
        fi
        TAG=$(echo "${LINE}" | cut -f 1 -d ":")
        VALUE=$(echo "${LINE}" | cut -f 2- -d " ")
        case $TAG in
        From)   FROM="${VALUE}";;
        Name)   NAME="${VALUE}";;
#        Path)   SUB_PATH="${ROOT_PATH}${VALUE}/";;
        Length) LENGTH="${VALUE}";;
        Type)   MIMETYPE="${VALUE}";;
        esac
done

case "${MODE}" in
put)
        FILE="${SUB_PATH}tmpimg.jpg"

        echo "script: testing ${FILE}..." 1>&2
        test -z "${FILE}" && exit 1
        /bin/rm -f "${FILE}"

        echo "OK"

        echo "script: storing file on disk..." 1>&2
        cat > "${FILE}"
        echo "script: file saved to disk." 1>&2

        echo "script: moving as ${NEW_NAME}" 1>&2
        /bin/mv "${FILE}" "${NEW_NAME}"

        /bin/rm -f "${FILE}"
        echo "script: done" 1>&2
        ;;

esac
exit 0
```