# Writeup

## Conocer la red

Lo primero de todos es encontrar equipos dentro de mi red.

`sudo arp-scan -I eth0 --localnet`



Ahora podemos saber cuál es la máquina atacante ya que estamos en VMware y la MAC siempre empieza por **00:0c** Por lo tanto sería la **192.168.95.101**

## Conectividad con la máquina atacante

Comprobamos que tenemos conectividad.

![captura-ping](https://github.com/Alv-fh/Vulnnyx_machines_writeups/assets/109484163/0b997875-f5bf-4ada-9c1f-6114f83be5cb)
