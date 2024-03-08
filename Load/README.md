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

Ahora que sabemos que tenemos conectividad, podemos escanear los puertos que estan abiertos para poder hacer la intrusión. Utilizamos la herramienta nmap

