![image](https://github.com/Alv-fh/Vulnnyx_machines_writeups/assets/109484163/e663c67b-2618-49ab-b089-fe21bddb5e07)

# Writeup
## Conocer la red

Hacemos mediante el comando `sudo arp-scan -I eth0 --localnet`

![image](https://github.com/Alv-fh/Vulnnyx_machines_writeups/assets/109484163/5182a9f5-61bc-4fdb-8ad2-3996c8f63913)

Vemos cuál es la IP porque todas las MAC de VirtualBox empiezan por **08:00**.

## Conectividad

Vemos el **TTL -> 64** que se trata de una máquina Linux.

![image](https://github.com/Alv-fh/Vulnnyx_machines_writeups/assets/109484163/628bc3ff-9a85-4a1c-82c1-f9a3bfaf7b96)

## Escaneo de puertos y servicios

Hacemos un escaneo con nmap y vemos que tiene abiertos los puertos:

![image](https://github.com/Alv-fh/Vulnnyx_machines_writeups/assets/109484163/8aae292d-bcd6-457e-ad84-319f3b93286a)

![image](https://github.com/Alv-fh/Vulnnyx_machines_writeups/assets/109484163/729726f1-756b-4649-a21a-15bcb22102a6)

