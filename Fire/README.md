# Writeup

## Conocer la red

Lo primero de todos es encontrar equipos dentro de mi red.

`sudo arp-scan -I eth0 --localnet`

![captura-red](https://github.com/Alv-fh/Vulnnyx_machines_writeups/assets/109484163/4e9f1b28-a21d-4768-9eeb-5f293d0107ee)

Hemos creado este script para hacerlo de manera más cómoda, **solo es efectivo si la máquina está en VMware o Virtualbox.**

![captura-script](https://github.com/Alv-fh/Vulnnyx_machines_writeups/assets/109484163/6566b47f-4c4c-4191-97c5-dfb3d367ec7f)

'#!/bin/bash

salida=`sudo arp-scan -I eth0 --localnet > arp.txt`
while read LINEA
do
        ip=`echo $LINEA | cut -f 1 -d "."`
        if [ $ip = "192" ] 2>/dev/null
        then
                mac=`echo $LINEA | cut -f 2 -d " "`
                identificar_mac1=`echo $mac | cut -f 1 -d ":"`
                identificar_mac2=`echo $mac | cut -f 2 -d ":"`
                if [ $identificar_mac1 == "00" ]
                then
                        if [ $identificar_mac2 = "0c" ]
                        then
                                ipfinal=`echo $LINEA | cut -f 1 -d " "`
                                echo "$ipfinal | $mac -> Vmare"
                        fi
                fi
                if [ $identificar_mac1 == "08" ]
                then
                        if [ $identificar_mac2 = "00" ]
                        then
                                ipfinal=`echo $LINEA | cut -f 1 -d " "`
                                echo "$ipfinal | $mac -> VirtualBox"
                        fi
                fi

        fi

done < arp.txt
sudo rm  arp.txt'
