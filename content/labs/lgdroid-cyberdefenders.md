+++
title = 'LGDroid - Cyberdefenders (Medium)'
+++

###### Category: Endpoint Forensics

[link del laboratorio LGDroid](https://cyberdefenders.org/blueteam-ctf-challenges/lgdroid/)

---

## Herramientas a utilizar

[DB Browser for SQLite](https://sqlitebrowser.org/)

---

## Unzip lab

```bash
Documents/labs/LGDroid 
❯ 7z x 69-LGDroid.zip -pcyberdefenders.org
```

```bash
Documents/labs/LGDroid 
❯ ls
69-LGDroid.zip  c51-OreoAnalyst/

Documents/labs/LGDroid 
❯ cd c51-OreoAnalyst/

labs/LGDroid/c51-OreoAnalyst 
❯ ls
'LGE LM-Q725K Quick Image.zip'   suspicious.jpg

labs/LGDroid/c51-OreoAnalyst 
❯ unzip LGE\ LM-Q725K\ Quick\ Image.zip 


labs/LGDroid/c51-OreoAnalyst 
❯ ls
 adb-data.tar  'LGE LM-Q725K Quick Image.zip'   sdcard.tar.gz            storage-sdcard1.tar.gz
'Agent Data'/  'Live Data'/                     storage-sdcard0.tar.gz   suspicious.jpg
```

---

## Preguntas

### Q1

What is the email address of Zoe Washburne?

¿Cuál es la dirección de correo electrónico de Zoe Washburne?

![contacts DB](/images/labs/cyberdefenders/lgdroid/contacts3db1.png) 

![contacts DB](/images/labs/cyberdefenders/lgdroid/contacts3db.png) 

![contacts DB](/images/labs/cyberdefenders/lgdroid/contacts3db2.png) 

#### Respuesta

`zoewash@0x42.null`


### Q2

What was the device time in UTC at the time of acquisition? (hh:mm:ss)

¿Cuál era la hora del dispositivo en UTC en el momento de la adquisición? (hh:mm:ss)

![UTC time](/images/labs/cyberdefenders/lgdroid/devicetime.png) 

#### Respuesta

`18:17:56`


### Q3

What time was Tor Browser downloaded in UTC? (hh:mm:ss)

¿A qué hora se descargó Tor Browser en UTC? (hh:mm:ss)

![downloads DB](/images/labs/cyberdefenders/lgdroid/downloadsdb.png) 

![downloads DB 1](/images/labs/cyberdefenders/lgdroid/downloadsdb1.png) 

![downloads DB 2](/images/labs/cyberdefenders/lgdroid/downloadsdb2.png) 

![downloads DB 3](/images/labs/cyberdefenders/lgdroid/downloadsdb3.png) 

#### Respuesta

`19:42:26`

### Q4

What time did the phone charge to 100% after the last reset? (hh:mm:ss)

¿A qué hora se cargó el teléfono al 100% después del último reinicio? (hh:mm:ss)

![battery](/images/labs/cyberdefenders/lgdroid/battery.png) 

![battery 1](/images/labs/cyberdefenders/lgdroid/battery1.png) 

```txt
13:12:19
00:05:01
--------
13:17:20
```

#### Respuesta

`13:17:20`


### Q5

What is the password for the most recently connected WIFI access point?

¿Cuál es la contraseña del último punto de acceso WIFI conectado?

![extractadbdata](/images/labs/cyberdefenders/lgdroid/extractadbdata.png) 

```bash
c51-OreoAnalyst/adb-data/apps 
❯ cd com.android.providers.settings/

adb-data/apps/com.android.providers.settings 
❯ ls
k/

adb-data/apps/com.android.providers.settings 
❯ cd k/

apps/com.android.providers.settings/k 
❯ ls
com.android.providers.settings.data*  _manifest


apps/com.android.providers.settings/k 
❯ cat com.android.providers.settings.data | grep -a "PreSharedKey"
<string name="PreSharedKey">&quot;ThinkingForest!&quot;</string>
```

#### Respuesta

`ThinkingForest!`

### Q6

What app was the user focused on at 2021-05-20 14:13:27?

¿En qué aplicación estaba concentrado el usuario el 2021-05-20 14:13:27?

```bash
LGDroid/c51-OreoAnalyst/Live Data 
❯ cat usage_stats.txt | grep "2021-05-20 14:13:27"
```

```bash
time="2021-05-20 14:13:27" type=MOVE_TO_BACKGROUND package=com.lge.launcher3 class=com.lge.launcher3.LauncherExtension flags=0x0 
time="2021-05-20 14:13:27" type=STANDBY_BUCKET_CHANGED package=com.google.android.youtube standbyBucket=10 reason=u-si flags=0x0 
time="2021-05-20 14:13:27" type=MOVE_TO_FOREGROUND package=com.google.android.youtube class=com.google.android.apps.youtube.app.application.Shell_HomeActivity flags=0x0 
time="2021-05-20 14:13:27" type=MOVE_TO_BACKGROUND package=com.google.android.youtube class=com.google.android.apps.youtube.app.application.Shell_HomeActivity flags=0x0 
time="2021-05-20 14:13:27" type=MOVE_TO_FOREGROUND package=com.google.android.youtube class=com.google.android.apps.youtube.app.watchwhile.WatchWhileActivity flags=0x0 
```

#### Respuesta

`youtube`

### Q7

How much time did the suspect watch Youtube on 2021-05-20? (hh:mm:ss)

¿Cuánto tiempo estuvo el sospechoso viendo Youtube el 2021-05-20? (hh:mm:ss)

```bash
LGDroid/c51-OreoAnalyst/Live Data 
❯ cat usage_stats.txt | grep "youtube" | grep "MOVE_TO_FOREGROUND" | tail -1
    time="2021-05-20 14:13:27" type=MOVE_TO_FOREGROUND package=com.google.android.youtube class=com.google.android.apps.youtube.app.watchwhile.WatchWhileActivity flags=0x0
```

```bash
LGDroid/c51-OreoAnalyst/Live Data 
❯ cat usage_stats.txt | grep "youtube" | grep "MOVE_TO_BACKGROUND" | tail -1
    time="2021-05-20 22:47:57" type=MOVE_TO_BACKGROUND package=com.google.android.youtube class=com.google.android.apps.youtube.app.watchwhile.WatchWhileActivity flags=0x0
```

```bash
22:47:57 - 14:13:27 -> 08:34:30
```

#### Respuesta

`08:34:30`


### Q8


"suspicious.jpg: What is the structural similarity metric for this image compared to a visually similar image taken with the mobile phone? (#.##).

"suspicious.jpg: ¿Cuál es la métrica de similitud estructural de esta imagen en comparación con una imagen visualmente similar tomada con el teléfono móvil? (#.##).

```bash
❯ tar -xvf sdcard.tar.gz

❯ cd sdcard/DCIM/Camera/

sdcard/DCIM/Camera 
❯ ls
20210429_151510.jpg  20210429_151804.jpg      20210429_152011.jpg      20210429_152209.jpg
20210429_151535.jpg  20210429_151826_HDR.jpg  20210429_152032_HDR.jpg  20210429_152221.jpg
20210429_151642.jpg  20210429_151858_HDR.jpg  20210429_152043.jpg      20210429_152224.jpg
20210429_151722.mp4  20210429_151943.jpg      20210429_152157.jpg      20210429_152321.jpg

sdcard/DCIM/Camera 
❯ thunar .
```

**20210429_151535.jpg es la más parecida a suspicious.jpg**

![similar pictures](/images/labs/cyberdefenders/lgdroid/similarpictures.png) 

[Image MS-SSIM — Image multi-scale structural similarity](https://darosh.github.io/image-ms-ssim-js/test/browser_test.html)

![MS-SSIM](/images/labs/cyberdefenders/lgdroid/ssim.png) 

#### Respuesta

`0.99`
