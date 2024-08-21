+++
title = 'AfricanFalls - Cyberdefenders (Medium)'
+++

###### Category: Endpoint Forensics

[link del laboratorio AfricanFalls](https://cyberdefenders.org/blueteam-ctf-challenges/africanfalls/)

---

## Herramientas a utilizar

[FTK Imager](https://www.exterro.com/digital-forensics-software/ftk-imager)

[hashes.com](https://hashes.com/en/decrypt/hash)

[Eric Zimmerman's tools](https://ericzimmerman.github.io/#!index.md) -> ShellBags Explorer

[mimikatz](https://github.com/gentilkiwi/mimikatz)

[DB Browser for SQLite](https://sqlitebrowser.org/)

[ExifTool](https://exiftool.org/)

---

## Unzip lab

 
![AfricanFalls unzip](/images/labs/cyberdefenders/africanfalls/africanfallsunzip.png)


![AfricanFalls unzip 2](/images/labs/cyberdefenders/africanfalls/africanfallsunzip2.png)


![AfricanFalls unzip 3](/images/labs/cyberdefenders/africanfalls/africanfallsunzip3.png)


![AfricanFalls unzip 4](/images/labs/cyberdefenders/africanfalls/africanfallsunzip4.png)


---

## Preguntas

### Q1


What is the MD5 hash value of the suspect disk?

¿Cuál es el hash MD5 del disco sospechoso?


![AfricanFalls 1](/images/labs/cyberdefenders/africanfalls/africanfalls1.png)


![AfricanFalls 2](/images/labs/cyberdefenders/africanfalls/africanfalls2.png)
 

#### Respuesta

`9471e69c95d8909ae60ddff30d50ffa1`


### Q2


What phrase did the suspect search for on 2021-04-29 18:17:38 UTC? (three words, two spaces in between)

¿Qué frase buscó el sospechoso el 2021-04-29 18:17:38 UTC? (tres palabras, dos espacios entre ellas)


```txt
Buscar en archivo [root]/Users/John Doe/AppData/Roaming/FileZilla/recentservers.xml 
```


![AfricanFalls 3](/images/labs/cyberdefenders/africanfalls/africanfalls3.png)


![AfricanFalls 5](/images/labs/cyberdefenders/africanfalls/africanfalls5.png)


![AfricanFalls 6](/images/labs/cyberdefenders/africanfalls/africanfalls6.png)

```py
import datetime

def date_from_webkit(webkit_timestamp):
    epoch_start = datetime.datetime(1601,1,1)
    delta = datetime.timedelta(microseconds=int(webkit_timestamp))
    result = epoch_start + delta

    if "2021-04-29 18:17:38" in str(result.replace(microsecond=0)):
       print(f"{webkit_timestamp} -> {result.replace(microsecond=0)}")

def convert_time():
    with open("last_visit_time", 'r') as last_times:
        for last_time in last_times:
            date_from_webkit(int(last_time))

convert_time()
```

```powershell
PS C:\Users\malsec\labs\66-Africanfalls\solution_files> py .\convert_time.py

13264193858900891 -> 2021-04-29 18:17:38
```

![AfricanFalls 4](/images/labs/cyberdefenders/africanfalls/africanfalls4.png)

#### Respuesta

`password cracking lists`


### Q3


What is the IPv4 address of the FTP server the suspect connected to?

¿Cuál es la dirección IPv4 del servidor FTP al que se conectó el sospechoso?

```txt
Buscar en archivo [root]/Users/John Doe/AppData/Roaming/FileZilla/recentservers.xml 
```

![AfricanFalls 7](/images/labs/cyberdefenders/africanfalls/africanfalls7.png)

#### Respuesta

`192[.]168[.]1[.]20`



### Q4


What date and time was a password list deleted in UTC? (YYYY-MM-DD HH:MM:SS UTC)

¿En qué fecha y hora se borró una lista de contraseñas en UTC? (AAAA-MM-DD HH:MM:SS UTC)


```txt
Buscar en [root]/$Recycle.Bin/S-1-5-21-3061953532-2461696977-1363062292-1001/
```

![AfricanFalls 8](/images/labs/cyberdefenders/africanfalls/africanfalls8.png)

#### Respuesta

`2021-04-29 18:22:17 UTC`


### Q5


How many times was Tor Browser ran on the suspect's computer? (number only)

¿Cuántas veces se ejecutó el Navegador Tor en el ordenador del sospechoso? (sólo número)


![AfricanFalls 9](/images/labs/cyberdefenders/africanfalls/africanfalls9.png)

**El navegador tor nunca se ejecutó, sólo se instaló.**

#### Respuesta

`0`


### Q6

What is the suspect's email address?

¿Cuál es la dirección de correo electrónico del sospechoso?


![AfricanFalls 10](/images/labs/cyberdefenders/africanfalls/africanfalls10.png)

#### Respuesta

`dreammaker82@protonmail.com`


### Q7


What is the FQDN did the suspect port scan?

```txt
Buscar en archivo [root]/Users/John Doe/AppData/Roaming/Microsoft/Windows/PowerShell/PSReadLine/ConsoleHost_history.txt
```

![AfricanFalls 11](/images/labs/cyberdefenders/africanfalls/africanfalls11.png)

#### Respuesta

`dfir.science`


### Q8


What country was picture "20210429_152043.jpg" allegedly taken in?

¿En qué país se tomó supuestamente la fotografía «20210429_152043.jpg»?


```txt
Buscar en directorio [root]/Users/John Doe/Pictures/Contact/ y exportar imagen para analizar con exiftool
```

```powershell
PS C:\Users\malsec\tools\exiftool-12.92_64> .\exiftool.exe C:\Users\malsec\labs\66-Africanfalls\exported\20210429_152043.jpg
```

```powershell
GPS Position : 16 deg 0' 0.00" S, 23 deg 0' 0.00" E
```

![AfricanFalls 12](/images/labs/cyberdefenders/africanfalls/africanfalls12.png)

#### Respuesta

`Zambia`


### Q9


What is the parent folder name picture "20210429_151535.jpg" was in before the suspect copy it to "contact" folder on his desktop?

¿Cuál es el nombre de la carpeta principal en la que estaba la foto «20210429_151535.jpg» antes de que el sospechoso la copiara en la carpeta «contactos» de su escritorio?


```powershell
PS C:\Users\malsec\tools\exiftool-12.92_64> .\exiftool.exe C:\Users\malsec\labs\66-Africanfalls\exported\20210429_151535.jpg
```

```powershell
Make : LG Electronics
Camera Model Name : LM-Q725K
```

```txt
Exportar archivo UsrClass.dat con FTK Imager y abrir con herramienta ShellBagsExplorer 
```

![AfricanFalls 13](/images/labs/cyberdefenders/africanfalls/africanfalls13.png)

> **Shellbags Explorer** es una herramienta con interfaz gráfica de usuario para explorar y analizar datos de shellbags. Los shellbags son un conjunto de claves de registro que contienen detalles sobre las carpetas visualizadas por un usuario, incluidos el tamaño, la posición y los iconos.


![AfricanFalls 14](/images/labs/cyberdefenders/africanfalls/africanfalls14.png)

#### Respuesta

`Camera`


### Q10


A Windows password hashes for an account are below. What is the user's password? Anon:1001:aad3b435b51404eeaad3b435b51404ee:3DE1A36F6DDB8E036DFD75E8E20C4AF4::: 

A continuación se muestra el hash de una contraseña de Windows para una cuenta. ¿Cuál es la contraseña del usuario? Anon:1001:aad3b435b51404eeaad3b435b51404ee:3DE1A36F6DDB8E036DFD75E8E20C4AF4:::

[hashes.com](https://hashes.com/en/decrypt/hash)


![AfricanFalls 15](/images/labs/cyberdefenders/africanfalls/africanfalls15.png)

```bash
3de1a36f6ddb8e036dfd75e8e20c4af4:AFR1CA!
```

#### Respuesta

`AFR1CA!`

### Q11

What is the user "John Doe's" Windows login password?

¿Cuál es la contraseña de acceso a Windows del usuario «John Doe»?

```txt
Exportar archivos SAM y SYSTEM ⇓⇓⇓   
```

![AfricanFalls 16](/images/labs/cyberdefenders/africanfalls/africanfalls16.png)


![AfricanFalls 17](/images/labs/cyberdefenders/africanfalls/africanfalls17.png)


```txt
Extracción de hash NTLM del usuario John Doe con mimikatz 
                          ⇓⇓⇓   
```

```powershell
PS C:\Users\malsec\tools\mimikatz_trunk\x64> .\mimikatz.exe "lsadump::sam /system:C:\Users\malsec\labs\66-Africanfalls\exported\SYSTEM /sam:C:\Users\malsec\labs\66-Africanfalls\exported\SAM"
```

```powershell
User : John Doe
  Hash NTLM: ecf53750b76cc9a62057ca85ff4c850e
```

![AfricanFalls 18](/images/labs/cyberdefenders/africanfalls/africanfalls18.png)

```bash
ecf53750b76cc9a62057ca85ff4c850e:ctf2021
```

#### Respuesta

`ctf2021`
