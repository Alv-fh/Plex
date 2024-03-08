# Writeup

## Conocer la red

Lo primero de todos es encontrar equipos dentro de mi red.

`sudo arp-scan -I eth0 --localnet`

![captura-arp](https://github.com/Alv-fh/Vulnnyx_machines_writeups/assets/109484163/7d2bd547-ed76-4c3c-80cb-1c780eb41418)

Ahora podemos saber cuál es la máquina atacante ya que estamos en VMware y la MAC siempre empieza por **00:0c** Por lo tanto sería la **192.168.95.101**

## Conectividad con la máquina atacante

Comprobamos que tenemos conectividad.

![captura-ping](https://github.com/Alv-fh/Vulnnyx_machines_writeups/assets/109484163/0b997875-f5bf-4ada-9c1f-6114f83be5cb)

Podemos ver que el **ttl** es **64** por lo tanto se trata de una máquina Linux, esto no es 100% fiable. Vemos que sí tenemos conectividad.

## Escaneo de puertos

Ahora que sabemos que tenemos conectividad, podemos escanear los puertos que estan abiertos para poder hacer la intrusión. Utilizamos la herramienta nmap.

`sudo nmap -p- -sS -sC -sV --min-rate=5000 -n -vvv -Pn 192.168.95.101 -oN allPorts`

Este comando lo que hace es buscar todos los puertos abiertos (1-65535) (`-p-`, lo hace de manera sigilosa (`-sS`), busca la versión de los servicios ( `-sV`), de manera rápida(`--min-rate=5000`), desactiva la resolución DNS para evitar problemas(`-n`), utiliza triple verbose para que tengamos más detalle del escaneo (`-vvv`)y omite la detección de host(`-Pn`). Lo guarda en el fichero allPorts (`-oN allPorts`).

![captura-nmap](https://github.com/Alv-fh/Vulnnyx_machines_writeups/assets/109484163/60784049-41c4-4519-b4d1-4a692d79fb6b)

Como podemos ver, están abiertos tanto el puerto **22 (SSH)** como el puerto **80 (HTTP)**. Por lo tanto podemos empezar a probar.

También vemos que utiliza **Apache 2.4.57**
También podemos ver que utiliza para el servivio **OpenSSH 9.2** para el servicio **SSH**.

![captura-apache](https://github.com/Alv-fh/Vulnnyx_machines_writeups/assets/109484163/abdd66c6-d05d-4439-abca-fd5b6f04debc)

Vemos que al buscar en el navegador web por la IP del atacante, nos devuelve la página por defecto de apache que viene al instalarse.

En este punto lo que se me ocurre es hacer un **fuzzing**, para conocer los directorios que hay. Para ello podemos utilizar la herramienta gobuster, o wfuzz. En mi caso voy a utilizar **gobuster**.

![captura-gobuster](https://github.com/Alv-fh/Vulnnyx_machines_writeups/assets/109484163/d075a291-bc97-4429-8512-0d00a42f6904)

Como podemos ver, encuentra el fichero **[robots.txt](https://es.wikipedia.org/wiki/Est%C3%A1ndar_de_exclusi%C3%B3n_de_robots)**, que ya hemos hablado en otras máquinas de él.

Ahora buscamos en el navegador el fichero **robots.txt** para conocer los directorios que están denegados o rechazados.

![captura-robots](https://github.com/Alv-fh/Vulnnyx_machines_writeups/assets/109484163/4c281690-8f0e-478e-a0c1-94aeccad7870)

Entramos en el directorio ritedev.

![captura-ritedev](https://github.com/Alv-fh/Vulnnyx_machines_writeups/assets/109484163/20ab4a7e-0058-486e-bfbb-27b6cd4b5c88)

Y se nos abre la esta página que a simple vista parece un estilo de tienda.

En este punto lo que se me ocurre es volver a hacer un ataque de diccionario **(Fuzzing)** para ver los directorios o archivos que hay.




