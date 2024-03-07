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

Como podemos ver dice que acepta las conexiones de puertos específicos y que las reenvia. 

![captura-sslh](https://github.com/Alv-fh/Plex/assets/109484163/52af3ff2-622d-4ab8-a7cc-b23027271ee4)

Entonces nosotros podemos hacer un **curl -I** para que solo nos muestre el **HEAD**, es decir la cabecera.

![captura-curl_-i](https://github.com/Alv-fh/Plex/assets/109484163/c393cf9a-6372-46ca-aa32-641453f06b2c)

Vemos que utiliza Apache2 por este puerto, entonces hacemos un **curl** de tipo **GET** para ver lo que responde.

![capura-get](https://github.com/Alv-fh/Plex/assets/109484163/98abd722-e30c-44f6-8439-3be1a584ebd4)

En este punto lo que se me ocurre es hacer fuzzing, para ello podemos utilizar tanto gobuster como wfuzz, en mi caso lo voy a hacer con wfuzz.








