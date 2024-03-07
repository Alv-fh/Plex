# Writeup

## Conocer la red

Lo primero de todos es encontrar equipos dentro de mi red.

`sudo arp-scan -I eth0 --localnet`

![captura-red](https://github.com/AlvarooFh/Plex/assets/148774363/f8868383-1b04-4df1-98d9-29cd3690c682)

Ahora podemos saber cuál es la máquina atacante ya que estamos en VMware y la MAC siempre empieza por **00:0c** Por lo tanto sería la **192.168.95.104**

## Conectividad con la máquina atacante

Comprobamos que tenemos conectividad.

`ping -c1 192.168.95.104`

![captura-conect](https://github.com/AlvarooFh/Plex/assets/148774363/94c0ee16-a204-4669-834f-52d790faa339)

Podemos ver que el **ttl** es **64** por lo tanto se trata de una máquina Linux, esto no es 100% fiable. Vemos que sí tenemos conectividad.

## Escaneo de puertos

Ahora que sabemos que tenemos conectividad, podemos escanear los puertos que estan abiertos para poder hacer la intrusión. Utilizamos la herramienta nmap

`sudo nmap -p- -sS -sC -sV --min-rate=5000 -n -vvv -Pn 192.168.95.104 -oN allPorts`

Este comando lo que hace es buscar todos los puertos abiertos (1-65535) (`-p-`, lo hace de manera sigilosa (`-sS`), busca la versión de los servicios ( `-sV`), de manera rápida(`--min-rate=5000`), desactiva la resolución DNS para evitar problemas(`-n`), utiliza triple verbose para que tengamos más detalle del escaneo (`-vvv`)y omite la detección de host(`-Pn`). Lo guarda en el fichero allPorts (`-oN allPorts`).

![captura-nmap](https://github.com/Alv-fh/Plex/assets/109484163/2c352940-0cee-4dd9-8c7b-0277b9a0aa3d)

Como podemos ver, el puerto 21 está abierto. Es el puerto de **FTP**. Pero hay algo raro, y es que si nos fijamos, en el servicio, pone **SSH**. En este caso parece una especie de **multiplexación**.
Comprobamos que efectivamente está utilizando el servicio SSH, ya que nos pide contraseña.

![captura-ssh](https://github.com/Alv-fh/Plex/assets/109484163/de4cf6cb-675d-42c8-bbc8-aae1bf0394c6)

Como no hay ningún otro puerto abierto, me da a pensar de que utiliza **SSLH** ya que usa distintos servicios por ese puerto.

[SSLH](https://github.com/yrutschle/sslh)
