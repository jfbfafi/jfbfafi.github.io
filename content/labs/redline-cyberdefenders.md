+++
title = 'RedLine - Cyberdefenders (Easy)'
+++


[link del laboratorio RedLine](https://cyberdefenders.org/blueteam-ctf-challenges/redline/)

## Herramientas a utilizar

- [Volatility 2](https://github.com/volatilityfoundation/volatility/releases/tag/2.6.1)
- [Volatility 3](https://github.com/volatilityfoundation/volatility3)

## Recursos 

[Volatility cheatsheet](https://blog.onfvp.com/post/volatility-cheatsheet/)


## Descargar laboratorio y moverlo a la máquina virtual REMnux

### unzip lab

```bash
7z x 106-RedLine.zip -pcyberdefenders.org
```

### Q1


What is the name of the suspicious process?

¿Cuál es el nombre del proceso sospechoso?

#### *windows info*

```bash
python3 vol.py -f /home/fafi/Documents/cyberdefenders/RedLine/MemoryDump.mem windows.info  
```

```bash  
Volatility 3 Framework 2.7.1
Progress:  100.00		PDB scanning finished                                                                                              
Variable	Value

Kernel Base	0xf8076221a000
DTB	0x1ad000
Symbols	file:///home/fafi/Repos/volatility3/volatility3/symbols/windows/ntkrnlmp.pdb/68A17FAF3012B7846079AEECDBE0A583-1.json.xz
Is64Bit	True
IsPAE	False
layer_name	0 WindowsIntel32e
memory_layer	1 FileLayer
KdVersionBlock	0xf80762e29398
Major/Minor	15.19041
MachineType	34404
KeNumberProcessors	4
SystemTime	2023-05-21 23:02:39
NtSystemRoot	C:\Windows
NtProductType	NtProductWinNt
NtMajorVersion	10
NtMinorVersion	0
PE MajorOperatingSystemVersion	10
PE MinorOperatingSystemVersion	0
PE Machine	34404
PE TimeDateStamp	Wed Jun 28 04:14:26 1995
```

#### *Process List*

```bash
python3 vol.py -f /home/fafi/Documents/cyberdefenders/RedLine/MemoryDump.mem windows.pslist > /home/fafi/Documents/cyberdefenders/RedLine/processlist
```

```bash
  
───────┬─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: processlist
───────┼─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ Volatility 3 Framework 2.7.1
   2   │ 
   3   │ PID PPID    ImageFileName   Offset(V)   Threads Handles SessionId   Wow64   CreateTime  ExitTime    File output
   4   │ 
   5   │ 4   0   System  0xad8185883180  157 -   N/A False   2023-05-21 22:27:10.000000  N/A Disabled
   6   │ 108 4   Registry    0xad81858f2080  4   -   N/A False   2023-05-21 22:26:54.000000  N/A Disabled
   7   │ 332 4   smss.exe    0xad81860dc040  2   -   N/A False   2023-05-21 22:27:10.000000  N/A Disabled
   8   │ 452 444 csrss.exe   0xad81861cd080  12  -   0   False   2023-05-21 22:27:22.000000  N/A Disabled
   9   │ 528 520 csrss.exe   0xad8186f1b140  14  -   1   False   2023-05-21 22:27:25.000000  N/A Disabled
  10   │ 552 444 wininit.exe 0xad8186f2b080  1   -   0   False   2023-05-21 22:27:25.000000  N/A Disabled
  11   │ 588 520 winlogon.exe    0xad8186f450c0  5   -   1   False   2023-05-21 22:27:25.000000  N/A Disabled
  12   │ 676 552 services.exe    0xad8186f4d080  7   -   0   False   2023-05-21 22:27:29.000000  N/A Disabled
  13   │ 696 552 lsass.exe   0xad8186fc6080  10  -   0   False   2023-05-21 22:27:29.000000  N/A Disabled
  14   │ 824 676 svchost.exe 0xad818761d240  22  -   0   False   2023-05-21 22:27:32.000000  N/A Disabled
  15   │ 852 552 fontdrvhost.ex  0xad818761b0c0  5   -   0   False   2023-05-21 22:27:33.000000  N/A Disabled
  16   │ 860 588 fontdrvhost.ex  0xad818761f140  5   -   1   False   2023-05-21 22:27:33.000000  N/A Disabled
  17   │ 952 676 svchost.exe 0xad81876802c0  12  -   0   False   2023-05-21 22:27:36.000000  N/A Disabled
  18   │ 1016    588 dwm.exe 0xad81876e4340  15  -   1   False   2023-05-21 22:27:38.000000  N/A Disabled
  19   │ 448 676 svchost.exe 0xad8187721240  54  -   0   False   2023-05-21 22:27:41.000000  N/A Disabled
  20   │ 752 676 svchost.exe 0xad8187758280  21  -   0   False   2023-05-21 22:27:43.000000  N/A Disabled
  21   │ 1012    676 svchost.exe 0xad818774c080  19  -   0   False   2023-05-21 22:27:43.000000  N/A Disabled
  22   │ 1196    676 svchost.exe 0xad81877972c0  34  -   0   False   2023-05-21 22:27:46.000000  N/A Disabled
  23   │ 1280    4   MemCompression  0xad8187835080  62  -   N/A False   2023-05-21 22:27:49.000000  N/A Disabled
  24   │ 1376    676 svchost.exe 0xad81878020c0  15  -   0   False   2023-05-21 22:27:49.000000  N/A Disabled
  25   │ 1448    676 svchost.exe 0xad818796c2c0  30  -   0   False   2023-05-21 22:27:52.000000  N/A Disabled
  26   │ 1496    676 svchost.exe 0xad81879752c0  12  -   0   False   2023-05-21 22:27:52.000000  N/A Disabled
  27   │ 1644    676 svchost.exe 0xad8187a112c0  6   -   0   False   2023-05-21 22:27:58.000000  N/A Disabled
  28   │ 1652    676 svchost.exe 0xad8187a2d2c0  10  -   0   False   2023-05-21 22:27:58.000000  N/A Disabled
  29   │ 1840    676 spoolsv.exe 0xad8187acb200  10  -   0   False   2023-05-21 22:28:03.000000  N/A Disabled
  30   │ 1892    676 svchost.exe 0xad8187b34080  14  -   0   False   2023-05-21 22:28:05.000000  N/A Disabled
  31   │ 2024    676 svchost.exe 0xad8187b65240  7   -   0   False   2023-05-21 22:28:11.000000  N/A Disabled
  32   │ 2076    676 svchost.exe 0xad8187b94080  10  -   0   False   2023-05-21 22:28:19.000000  N/A Disabled
  33   │ 2144    676 vmtoolsd.exe    0xad81896ab080  11  -   0   False   2023-05-21 22:28:19.000000  N/A Disabled
  34   │ 2152    676 vm3dservice.ex  0xad81896ae240  2   -   0   False   2023-05-21 22:28:19.000000  N/A Disabled
  35   │ 2200    676 VGAuthService.  0xad81896b3300  2   -   0   False   2023-05-21 22:28:19.000000  N/A Disabled
  36   │ 2404    2152    vm3dservice.ex  0xad8186619200  2   -   1   False   2023-05-21 22:28:32.000000  N/A Disabled
  37   │ 3028    676 dllhost.exe 0xad8185907080  12  -   0   False   2023-05-21 22:29:20.000000  N/A Disabled
  38   │ 832 676 msdtc.exe   0xad8185861280  9   -   0   False   2023-05-21 22:29:25.000000  N/A Disabled
  39   │ 1232    676 svchost.exe 0xad8186f4a2c0  7   -   0   False   2023-05-21 22:29:39.000000  N/A Disabled
  40   │ 1392    448 sihost.exe  0xad8189e94280  11  -   1   False   2023-05-21 22:30:08.000000  N/A Disabled
  41   │ 1064    676 svchost.exe 0xad8189d7c2c0  15  -   1   False   2023-05-21 22:30:09.000000  N/A Disabled
  42   │ 1600    448 taskhostw.exe   0xad8189d07300  10  -   1   False   2023-05-21 22:30:09.000000  N/A Disabled
  43   │ 3204    752 ctfmon.exe  0xad8189c8b280  12  -   1   False   2023-05-21 22:30:11.000000  N/A Disabled
  44   │ 3556    588 userinit.exe    0xad818c02f340  0   -   1   False   2023-05-21 22:30:28.000000  2023-05-21 22:30:43.000000  Disabled
  45   │ 3580    3556    explorer.exe    0xad818c047340  76  -   1   False   2023-05-21 22:30:28.000000  N/A Disabled
  46   │ 3944    824 WmiPrvSE.exe    0xad818c054080  13  -   0   False   2023-05-21 22:30:44.000000  N/A Disabled
  47   │ 3004    676 svchost.exe 0xad818c4212c0  7   -   0   False   2023-05-21 22:30:55.000000  N/A Disabled
  48   │ 1116    676 svchost.exe 0xad818c426080  6   -   1   False   2023-05-21 22:31:00.000000  N/A Disabled
  49   │ 3160    824 StartMenuExper  0xad818cad3240  14  -   1   False   2023-05-21 22:31:21.000000  N/A Disabled
  50   │ 4116    824 RuntimeBroker.  0xad818cd93300  3   -   1   False   2023-05-21 22:31:24.000000  N/A Disabled
  51   │ 4228    676 SearchIndexer.  0xad818ce06240  15  -   0   False   2023-05-21 22:31:27.000000  N/A Disabled
  52   │ 4448    824 RuntimeBroker.  0xad818c09a080  9   -   1   False   2023-05-21 22:31:33.000000  N/A Disabled
  53   │ 464 3580    SecurityHealth  0xad818979d080  3   -   1   False   2023-05-21 22:31:59.000000  N/A Disabled
  54   │ 3252    3580    vmtoolsd.exe    0xad8189796300  8   -   1   False   2023-05-21 22:31:59.000000  N/A Disabled
  55   │ 5136    676 SecurityHealth  0xad818d374280  7   -   0   False   2023-05-21 22:32:01.000000  N/A Disabled
  56   │ 5328    3580    msedge.exe  0xad818d0980c0  54  -   1   False   2023-05-21 22:32:02.000000  N/A Disabled
  57   │ 4396    5328    msedge.exe  0xad818d515080  7   -   1   False   2023-05-21 22:32:19.000000  N/A Disabled
  58   │ 1144    5328    msedge.exe  0xad818d75f080  18  -   1   False   2023-05-21 22:32:38.000000  N/A Disabled
  59   │ 4544    5328    msedge.exe  0xad818d75b080  14  -   1   False   2023-05-21 22:32:39.000000  N/A Disabled
  60   │ 5340    5328    msedge.exe  0xad818d7b3080  10  -   1   False   2023-05-21 22:32:39.000000  N/A Disabled
  61   │ 5704    824 RuntimeBroker.  0xad8185962080  5   -   1   False   2023-05-21 22:32:44.000000  N/A Disabled
  62   │ 1764    824 dllhost.exe 0xad818d176080  7   -   1   False   2023-05-21 22:32:48.000000  N/A Disabled
  63   │ 1916    824 SearchApp.exe   0xad818d099080  24  -   1   False   2023-05-21 22:33:05.000000  N/A Disabled
  64   │ 6200    676 SgrmBroker.exe  0xad818d09f080  7   -   0   False   2023-05-21 22:33:42.000000  N/A Disabled
  65   │ 6696    676 svchost.exe 0xad818c532080  8   -   0   False   2023-05-21 22:34:07.000000  N/A Disabled
  66   │ 7312    824 ApplicationFra  0xad818e84f300  10  -   1   False   2023-05-21 22:35:44.000000  N/A Disabled
  67   │ 7772    676 svchost.exe 0xad818e88e140  3   -   0   False   2023-05-21 22:36:03.000000  N/A Disabled
  68   │ 6724    3580    Outline.exe 0xad818e578080  0   -   1   True    2023-05-21 22:36:09.000000  2023-05-21 23:01:24.000000  Disabled
  69   │ 4224    6724    Outline.exe 0xad818e88b080  0   -   1   True    2023-05-21 22:36:23.000000  2023-05-21 23:01:24.000000  Disabled
  70   │ 7160    824 SearchApp.exe   0xad818ccc4080  57  -   1   False   2023-05-21 22:39:13.000000  N/A Disabled
  71   │ 4628    6724    tun2socks.exe   0xad818de82340  0   -   1   True    2023-05-21 22:40:10.000000  2023-05-21 23:01:24.000000  Disabled
  72   │ 6048    448 taskhostw.exe   0xad818dc5d080  5   -   1   False   2023-05-21 22:40:20.000000  N/A Disabled
  73   │ 8264    824 RuntimeBroker.  0xad818eec8080  4   -   1   False   2023-05-21 22:40:33.000000  N/A Disabled
  74   │ 3608    676 svchost.exe 0xad818d07a080  3   -   0   False   2023-05-21 22:41:28.000000  N/A Disabled
  75   │ 6644    824 SkypeApp.exe    0xad818d3ac080  49  -   1   False   2023-05-21 22:41:52.000000  N/A Disabled
  76   │ 5656    824 RuntimeBroker.  0xad81876e8080  0   -   1   False   2023-05-21 21:58:19.000000  2023-05-21 22:02:01.000000  Disabled
  77   │ 8952    824 TextInputHost.  0xad818e6db080  10  -   1   False   2023-05-21 21:59:11.000000  N/A Disabled
  78   │ 5808    824 HxTsr.exe   0xad818de5d080  0   -   1   False   2023-05-21 21:59:58.000000  2023-05-21 22:07:45.000000  Disabled
  79   │ 2388    5328    msedge.exe  0xad818e54c340  18  -   1   False   2023-05-21 22:05:35.000000  N/A Disabled
  80   │ 6292    5328    msedge.exe  0xad818d7a1080  20  -   1   False   2023-05-21 22:06:15.000000  N/A Disabled
  81   │ 3876    448 taskhostw.exe   0xad8189b30080  8   -   1   False   2023-05-21 22:08:02.000000  N/A Disabled
  82   │ 372 824 SkypeBackgroun  0xad8186f49080  3   -   1   False   2023-05-21 22:10:00.000000  N/A Disabled
  83   │ 1120    676 MsMpEng.exe 0xad818945c080  12  -   0   False   2023-05-21 22:10:01.000000  N/A Disabled
  84   │ 6076    824 ShellExperienc  0xad818eb18080  14  -   1   False   2023-05-21 22:11:36.000000  N/A Disabled
  85   │ 7336    824 RuntimeBroker.  0xad818e8bb080  2   -   1   False   2023-05-21 22:11:39.000000  N/A Disabled
  86   │ 7964    5328    msedge.exe  0xad818dee5080  19  -   1   False   2023-05-21 22:22:09.000000  N/A Disabled
  87   │ 6544    5328    msedge.exe  0xad818c0ea080  18  -   1   False   2023-05-21 22:22:35.000000  N/A Disabled
  88   │ 5964    676 svchost.exe 0xad818ef86080  5   -   0   False   2023-05-21 22:27:56.000000  N/A Disabled
  89   │ 8896    5328    msedge.exe  0xad8187a39080  18  -   1   False   2023-05-21 22:28:21.000000  N/A Disabled
  90   │ 5156    5328    msedge.exe  0xad818c553080  14  -   1   False   2023-05-21 22:28:22.000000  N/A Disabled
  91   │ 5896    8844    oneetx.exe  0xad8189b41080  5   -   1   True    2023-05-21 22:30:56.000000  N/A Disabled
  92   │ 7732    5896    rundll32.exe    0xad818d1912c0  1   -   1   True    2023-05-21 22:31:53.000000  N/A Disabled
  93   │ 6324    1496    audiodg.exe 0xad818df2e080  4   -   0   False   2023-05-21 22:42:56.000000  N/A Disabled
  94   │ 2228    3580    FTK Imager.exe  0xad818d143080  10  -   1   False   2023-05-21 22:43:56.000000  N/A Disabled
  95   │ 5636    3580    notepad.exe 0xad818db45080  1   -   1   False   2023-05-21 22:46:50.000000  N/A Disabled
  96   │ 2044    676 svchost.exe 0xad8189b27080  28  -   0   False   2023-05-21 22:49:29.000000  N/A Disabled
  97   │ 8708    676 svchost.exe 0xad818d431080  5   -   0   False   2023-05-21 22:57:33.000000  N/A Disabled
  98   │ 5476    676 svchost.exe 0xad818e752080  9   -   0   False   2023-05-21 22:58:08.000000  N/A Disabled
  99   │ 6596    676 TrustedInstall  0xad818dc88080  4   -   0   False   2023-05-21 22:58:13.000000  N/A Disabled
 100   │ 2332    824 TiWorker.exe    0xad818e780080  4   -   0   False   2023-05-21 22:58:13.000000  N/A Disabled
 101   │ 4340    676 VSSVC.exe   0xad818e888080  3   -   0   False   2023-05-21 23:01:06.000000  N/A Disabled
 102   │ 7540    824 smartscreen.ex  0xad818e893080  14  -   1   False   2023-05-21 23:02:26.000000  N/A Disabled
 103   │ 8920    3580    FTK Imager.exe  0xad818ef81080  20  -   1   False   2023-05-21 23:02:28.000000  N/A Disabled
 104   │ 5480    448 oneetx.exe  0xad818d3d6080  6   -   1   True    2023-05-21 23:03:00.000000  N/A Disabled
───────┴─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
```

#### Respuesta

`oneetx.exe`


### Q2

What is the child process name of the suspicious process?

¿Cuál es el nombre del proceso hijo del proceso sospechoso?

#### *Process tree*

```bash  
python3 vol.py -f /home/fafi/Documents/cyberdefenders/RedLine/MemoryDump.mem windows.pstree > /home/fafi/Documents/cyberdefenders/RedLine/processtree
```

```bash  
───────┬─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: processtree
───────┼─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ Volatility 3 Framework 2.7.1
   2   │ 
   3   │ PID PPID    ImageFileName   Offset(V)   Threads Handles SessionId   Wow64   CreateTime  ExitTime    Audit   Cmd Path
   4   │ 
   5   │ 4   0   System  0xad8185883180  157 -   N/A False   2023-05-21 22:27:10.000000  N/A -   -   -
   6   │ * 1280  4   MemCompression  0xad8187835080  62  -   N/A False   2023-05-21 22:27:49.000000  N/A MemCompression  -   -
   7   │ * 108   4   Registry    0xad81858f2080  4   -   N/A False   2023-05-21 22:26:54.000000  N/A Registry    -   -
   8   │ * 332   4   smss.exe    0xad81860dc040  2   -   N/A False   2023-05-21 22:27:10.000000  N/A \Device\HarddiskVolume3\Windows\System32\smss.ex
       │ e   -   -
   9   │ 452 444 csrss.exe   0xad81861cd080  12  -   0   False   2023-05-21 22:27:22.000000  N/A \Device\HarddiskVolume3\Windows\System32\csrss.exe  
       │ -   -
  10   │ 528 520 csrss.exe   0xad8186f1b140  14  -   1   False   2023-05-21 22:27:25.000000  N/A \Device\HarddiskVolume3\Windows\System32\csrss.exe  
       │     
  11   │ 552 444 wininit.exe 0xad8186f2b080  1   -   0   False   2023-05-21 22:27:25.000000  N/A \Device\HarddiskVolume3\Windows\System32\wininit.exe
       │     -   -
  12   │ * 696   552 lsass.exe   0xad8186fc6080  10  -   0   False   2023-05-21 22:27:29.000000  N/A \Device\HarddiskVolume3\Windows\System32\lsass.e
       │ xe  C:\Windows\system32\lsass.exe   C:\Windows\system32\lsass.exe
  13   │ * 676   552 services.exe    0xad8186f4d080  7   -   0   False   2023-05-21 22:27:29.000000  N/A \Device\HarddiskVolume3\Windows\System32\ser
       │ vices.exe   C:\Windows\system32\services.exe    C:\Windows\system32\services.exe
  14   │ ** 4228 676 SearchIndexer.  0xad818ce06240  15  -   0   False   2023-05-21 22:31:27.000000  N/A \Device\HarddiskVolume3\Windows\System32\Sea
       │ rchIndexer.exe  C:\Windows\system32\SearchIndexer.exe /Embedding    C:\Windows\system32\SearchIndexer.exe
  15   │ ** 8708 676 svchost.exe 0xad818d431080  5   -   0   False   2023-05-21 22:57:33.000000  N/A \Device\HarddiskVolume3\Windows\System32\svchost
       │ .exe    -   -
  16   │ ** 5136 676 SecurityHealth  0xad818d374280  7   -   0   False   2023-05-21 22:32:01.000000  N/A \Device\HarddiskVolume3\Windows\System32\Sec
       │ urityHealthService.exe  -   -
  17   │ ** 2200 676 VGAuthService.  0xad81896b3300  2   -   0   False   2023-05-21 22:28:19.000000  N/A \Device\HarddiskVolume3\Program Files\VMware
       │ \VMware Tools\VMware VGAuth\VGAuthService.exe   -   -
  18   │ ** 3608 676 svchost.exe 0xad818d07a080  3   -   0   False   2023-05-21 22:41:28.000000  N/A \Device\HarddiskVolume3\Windows\System32\svchost
       │ .exe    -   -
  19   │ ** 2076 676 svchost.exe 0xad8187b94080  10  -   0   False   2023-05-21 22:28:19.000000  N/A \Device\HarddiskVolume3\Windows\System32\svchost
       │ .exe    C:\Windows\System32\svchost.exe -k utcsvc -p    C:\Windows\System32\svchost.exe
  20   │ ** 1448 676 svchost.exe 0xad818796c2c0  30  -   0   False   2023-05-21 22:27:52.000000  N/A \Device\HarddiskVolume3\Windows\System32\svchost
       │ .exe    C:\Windows\System32\svchost.exe -k NetworkService -p    C:\Windows\System32\svchost.exe
  21   │ ** 1064 676 svchost.exe 0xad8189d7c2c0  15  -   1   False   2023-05-21 22:30:09.000000  N/A \Device\HarddiskVolume3\Windows\System32\svchost
       │ .exe    C:\Windows\system32\svchost.exe -k UnistackSvcGroup C:\Windows\system32\svchost.exe
  22   │ ** 6696 676 svchost.exe 0xad818c532080  8   -   0   False   2023-05-21 22:34:07.000000  N/A \Device\HarddiskVolume3\Windows\System32\svchost
       │ .exe    -   -
  23   │ ** 1196 676 svchost.exe 0xad81877972c0  34  -   0   False   2023-05-21 22:27:46.000000  N/A \Device\HarddiskVolume3\Windows\System32\svchost
       │ .exe    C:\Windows\system32\svchost.exe -k LocalService -p  C:\Windows\system32\svchost.exe
  24   │ ** 1840 676 spoolsv.exe 0xad8187acb200  10  -   0   False   2023-05-21 22:28:03.000000  N/A \Device\HarddiskVolume3\Windows\System32\spoolsv
       │ .exe    -   -
  25   │ ** 952  676 svchost.exe 0xad81876802c0  12  -   0   False   2023-05-21 22:27:36.000000  N/A \Device\HarddiskVolume3\Windows\System32\svchost
       │ .exe    C:\Windows\system32\svchost.exe -k RPCSS -p C:\Windows\system32\svchost.exe
  26   │ ** 824  676 svchost.exe 0xad818761d240  22  -   0   False   2023-05-21 22:27:32.000000  N/A \Device\HarddiskVolume3\Windows\System32\svchost
       │ .exe    C:\Windows\system32\svchost.exe -k DcomLaunch -p    C:\Windows\system32\svchost.exe
  27   │ *** 7312    824 ApplicationFra  0xad818e84f300  10  -   1   False   2023-05-21 22:35:44.000000  N/A \Device\HarddiskVolume3\Windows\System32
       │ \ApplicationFrameHost.exe   C:\Windows\system32\ApplicationFrameHost.exe -Embedding C:\Windows\system32\ApplicationFrameHost.exe
  28   │ *** 4116    824 RuntimeBroker.  0xad818cd93300  3   -   1   False   2023-05-21 22:31:24.000000  N/A \Device\HarddiskVolume3\Windows\System32
       │ \RuntimeBroker.exe  -   -
  29   │ *** 5656    824 RuntimeBroker.  0xad81876e8080  0   -   1   False   2023-05-21 21:58:19.000000  2023-05-21 22:02:01.000000  \Device\Harddisk
       │ Volume3\Windows\System32\RuntimeBroker.exe  -   -
  30   │ *** 2332    824 TiWorker.exe    0xad818e780080  4   -   0   False   2023-05-21 22:58:13.000000  N/A \Device\HarddiskVolume3\Windows\WinSxS\a
       │ md64_microsoft-windows-servicingstack_31bf3856ad364e35_10.0.19041.1940_none_7dd80d767cb5c7b0\TiWorker.exe   -   -
  31   │ *** 7336    824 RuntimeBroker.  0xad818e8bb080  2   -   1   False   2023-05-21 22:11:39.000000  N/A \Device\HarddiskVolume3\Windows\System32
       │ \RuntimeBroker.exe  -   -
  32   │ *** 5808    824 HxTsr.exe   0xad818de5d080  0   -   1   False   2023-05-21 21:59:58.000000  2023-05-21 22:07:45.000000  \Device\HarddiskVolu
       │ me3\Program Files\WindowsApps\microsoft.windowscommunicationsapps_16005.11629.20316.0_x64__8wekyb3d8bbwe\HxTsr.exe  -   -
  33   │ *** 7160    824 SearchApp.exe   0xad818ccc4080  57  -   1   False   2023-05-21 22:39:13.000000  N/A \Device\HarddiskVolume3\Windows\SystemAp
       │ ps\Microsoft.Windows.Search_cw5n1h2txyewy\SearchApp.exe -   -
  34   │ *** 6076    824 ShellExperienc  0xad818eb18080  14  -   1   False   2023-05-21 22:11:36.000000  N/A \Device\HarddiskVolume3\Windows\SystemAp
       │ ps\ShellExperienceHost_cw5n1h2txyewy\ShellExperienceHost.exe    -   -
  35   │ *** 5704    824 RuntimeBroker.  0xad8185962080  5   -   1   False   2023-05-21 22:32:44.000000  N/A \Device\HarddiskVolume3\Windows\System32
       │ \RuntimeBroker.exe  C:\Windows\System32\RuntimeBroker.exe -Embedding    C:\Windows\System32\RuntimeBroker.exe
  36   │ *** 8264    824 RuntimeBroker.  0xad818eec8080  4   -   1   False   2023-05-21 22:40:33.000000  N/A \Device\HarddiskVolume3\Windows\System32
       │ \RuntimeBroker.exe  -   -
  37   │ *** 3160    824 StartMenuExper  0xad818cad3240  14  -   1   False   2023-05-21 22:31:21.000000  N/A \Device\HarddiskVolume3\Windows\SystemAp
       │ ps\Microsoft.Windows.StartMenuExperienceHost_cw5n1h2txyewy\StartMenuExperienceHost.exe  "C:\Windows\SystemApps\Microsoft.Windows.StartMenuEx
       │ perienceHost_cw5n1h2txyewy\StartMenuExperienceHost.exe" -ServerName:App.AppXywbrabmsek0gm3tkwpr5kwzbs55tkqay.mca    C:\Windows\SystemApps\Mi
       │ crosoft.Windows.StartMenuExperienceHost_cw5n1h2txyewy\StartMenuExperienceHost.exe
  38   │ *** 4448    824 RuntimeBroker.  0xad818c09a080  9   -   1   False   2023-05-21 22:31:33.000000  N/A \Device\HarddiskVolume3\Windows\System32
       │ \RuntimeBroker.exe  C:\Windows\System32\RuntimeBroker.exe -Embedding    C:\Windows\System32\RuntimeBroker.exe
  39   │ *** 1764    824 dllhost.exe 0xad818d176080  7   -   1   False   2023-05-21 22:32:48.000000  N/A \Device\HarddiskVolume3\Windows\System32\dll
       │ host.exe        
  40   │ *** 3944    824 WmiPrvSE.exe    0xad818c054080  13  -   0   False   2023-05-21 22:30:44.000000  N/A \Device\HarddiskVolume3\Windows\System32
       │ \wbem\WmiPrvSE.exe  C:\Windows\system32\wbem\wmiprvse.exe   C:\Windows\system32\wbem\wmiprvse.exe
  41   │ *** 6644    824 SkypeApp.exe    0xad818d3ac080  49  -   1   False   2023-05-21 22:41:52.000000  N/A \Device\HarddiskVolume3\Program Files\Wi
       │ ndowsApps\Microsoft.SkypeApp_14.53.77.0_x64__kzf8qxf38zg5c\SkypeApp.exe -   -
  42   │ *** 372 824 SkypeBackgroun  0xad8186f49080  3   -   1   False   2023-05-21 22:10:00.000000  N/A \Device\HarddiskVolume3\Program Files\Window
       │ sApps\Microsoft.SkypeApp_14.53.77.0_x64__kzf8qxf38zg5c\SkypeBackgroundHost.exe  -   -
  43   │ *** 7540    824 smartscreen.ex  0xad818e893080  14  -   1   False   2023-05-21 23:02:26.000000  N/A \Device\HarddiskVolume3\Windows\System32
       │ \smartscreen.exe    C:\Windows\System32\smartscreen.exe -Embedding  C:\Windows\System32\smartscreen.exe
  44   │ *** 8952    824 TextInputHost.  0xad818e6db080  10  -   1   False   2023-05-21 21:59:11.000000  N/A \Device\HarddiskVolume3\Windows\SystemAp
       │ ps\MicrosoftWindows.Client.CBS_cw5n1h2txyewy\TextInputHost.exe  "C:\Windows\SystemApps\MicrosoftWindows.Client.CBS_cw5n1h2txyewy\TextInputHo
       │ st.exe" -ServerName:InputApp.AppXjd5de1g66v206tj52m9d0dtpppx4cgpn.mca   C:\Windows\SystemApps\MicrosoftWindows.Client.CBS_cw5n1h2txyewy\Text
       │ InputHost.exe
  45   │ *** 1916    824 SearchApp.exe   0xad818d099080  24  -   1   False   2023-05-21 22:33:05.000000  N/A \Device\HarddiskVolume3\Windows\SystemAp
       │ ps\Microsoft.Windows.Search_cw5n1h2txyewy\SearchApp.exe -   -
  46   │ ** 6200 676 SgrmBroker.exe  0xad818d09f080  7   -   0   False   2023-05-21 22:33:42.000000  N/A \Device\HarddiskVolume3\Windows\System32\Sgr
       │ mBroker.exe -   -
  47   │ ** 3004 676 svchost.exe 0xad818c4212c0  7   -   0   False   2023-05-21 22:30:55.000000  N/A \Device\HarddiskVolume3\Windows\System32\svchost
       │ .exe    C:\Windows\system32\svchost.exe -k LocalServiceAndNoImpersonation -p    C:\Windows\system32\svchost.exe
  48   │ ** 448  676 svchost.exe 0xad8187721240  54  -   0   False   2023-05-21 22:27:41.000000  N/A \Device\HarddiskVolume3\Windows\System32\svchost
       │ .exe    C:\Windows\system32\svchost.exe -k netsvcs -p   C:\Windows\system32\svchost.exe
  49   │ *** 1600    448 taskhostw.exe   0xad8189d07300  10  -   1   False   2023-05-21 22:30:09.000000  N/A \Device\HarddiskVolume3\Windows\System32
       │ \taskhostw.exe  -   -
  50   │ *** 6048    448 taskhostw.exe   0xad818dc5d080  5   -   1   False   2023-05-21 22:40:20.000000  N/A \Device\HarddiskVolume3\Windows\System32
       │ \taskhostw.exe  -   -
  51   │ *** 3876    448 taskhostw.exe   0xad8189b30080  8   -   1   False   2023-05-21 22:08:02.000000  N/A \Device\HarddiskVolume3\Windows\System32
       │ \taskhostw.exe  -   -
  52   │ *** 5480    448 oneetx.exe  0xad818d3d6080  6   -   1   True    2023-05-21 23:03:00.000000  N/A \Device\HarddiskVolume3\Users\Tammam\AppData
       │ \Local\Temp\c3912af058\oneetx.exe   -   -
  53   │ *** 1392    448 sihost.exe  0xad8189e94280  11  -   1   False   2023-05-21 22:30:08.000000  N/A \Device\HarddiskVolume3\Windows\System32\sih
       │ ost.exe sihost.exe  C:\Windows\system32\sihost.exe
  54   │ ** 832  676 msdtc.exe   0xad8185861280  9   -   0   False   2023-05-21 22:29:25.000000  N/A \Device\HarddiskVolume3\Windows\System32\msdtc.e
       │ xe  -   -
  55   │ ** 6596 676 TrustedInstall  0xad818dc88080  4   -   0   False   2023-05-21 22:58:13.000000  N/A \Device\HarddiskVolume3\Windows\servicing\Tr
       │ ustedInstaller.exe  -   -
  56   │ ** 5964 676 svchost.exe 0xad818ef86080  5   -   0   False   2023-05-21 22:27:56.000000  N/A \Device\HarddiskVolume3\Windows\System32\svchost
       │ .exe    -   -
  57   │ ** 1232 676 svchost.exe 0xad8186f4a2c0  7   -   0   False   2023-05-21 22:29:39.000000  N/A \Device\HarddiskVolume3\Windows\System32\svchost
       │ .exe    -   -
  58   │ ** 3028 676 dllhost.exe 0xad8185907080  12  -   0   False   2023-05-21 22:29:20.000000  N/A \Device\HarddiskVolume3\Windows\System32\dllhost
       │ .exe    C:\Windows\system32\dllhost.exe /Processid:{02D4B3F1-FD88-11D1-960D-00805FC79235}   C:\Windows\system32\dllhost.exe
  59   │ ** 1496 676 svchost.exe 0xad81879752c0  12  -   0   False   2023-05-21 22:27:52.000000  N/A \Device\HarddiskVolume3\Windows\System32\svchost
       │ .exe    C:\Windows\System32\svchost.exe -k LocalServiceNetworkRestricted -p C:\Windows\System32\svchost.exe
  60   │ *** 6324    1496    audiodg.exe 0xad818df2e080  4   -   0   False   2023-05-21 22:42:56.000000  N/A \Device\HarddiskVolume3\Windows\System32
       │ \audiodg.exe    -   -
  61   │ ** 1116 676 svchost.exe 0xad818c426080  6   -   1   False   2023-05-21 22:31:00.000000  N/A \Device\HarddiskVolume3\Windows\System32\svchost
       │ .exe    C:\Windows\system32\svchost.exe -k ClipboardSvcGroup -p C:\Windows\system32\svchost.exe
  62   │ ** 7772 676 svchost.exe 0xad818e88e140  3   -   0   False   2023-05-21 22:36:03.000000  N/A \Device\HarddiskVolume3\Windows\System32\svchost
       │ .exe    -   -
  63   │ ** 1376 676 svchost.exe 0xad81878020c0  15  -   0   False   2023-05-21 22:27:49.000000  N/A \Device\HarddiskVolume3\Windows\System32\svchost
       │ .exe    C:\Windows\system32\svchost.exe -k LocalServiceNoNetwork -p C:\Windows\system32\svchost.exe
  64   │ ** 2144 676 vmtoolsd.exe    0xad81896ab080  11  -   0   False   2023-05-21 22:28:19.000000  N/A \Device\HarddiskVolume3\Program Files\VMware
       │ \VMware Tools\vmtoolsd.exe  "C:\Program Files\VMware\VMware Tools\vmtoolsd.exe" C:\Program Files\VMware\VMware Tools\vmtoolsd.exe
  65   │ ** 1120 676 MsMpEng.exe 0xad818945c080  12  -   0   False   2023-05-21 22:10:01.000000  N/A \Device\HarddiskVolume3\ProgramData\Microsoft\Wi
       │ ndows Defender\Platform\4.18.2304.8-0\MsMpEng.exe       
  66   │ ** 1892 676 svchost.exe 0xad8187b34080  14  -   0   False   2023-05-21 22:28:05.000000  N/A \Device\HarddiskVolume3\Windows\System32\svchost
       │ .exe    C:\Windows\system32\svchost.exe -k LocalServiceNoNetworkFirewall -p C:\Windows\system32\svchost.exe
  67   │ ** 5476 676 svchost.exe 0xad818e752080  9   -   0   False   2023-05-21 22:58:08.000000  N/A \Device\HarddiskVolume3\Windows\System32\svchost
       │ .exe    C:\Windows\System32\svchost.exe -k NetworkService -p    C:\Windows\System32\svchost.exe
  68   │ ** 2024 676 svchost.exe 0xad8187b65240  7   -   0   False   2023-05-21 22:28:11.000000  N/A \Device\HarddiskVolume3\Windows\System32\svchost
       │ .exe    -   -
  69   │ ** 2152 676 vm3dservice.ex  0xad81896ae240  2   -   0   False   2023-05-21 22:28:19.000000  N/A \Device\HarddiskVolume3\Windows\System32\vm3
       │ dservice.exe    -   -
  70   │ *** 2404    2152    vm3dservice.ex  0xad8186619200  2   -   1   False   2023-05-21 22:28:32.000000  N/A \Device\HarddiskVolume3\Windows\Syst
       │ em32\vm3dservice.exe    -   -
  71   │ ** 1644 676 svchost.exe 0xad8187a112c0  6   -   0   False   2023-05-21 22:27:58.000000  N/A \Device\HarddiskVolume3\Windows\System32\svchost
       │ .exe    -   -
  72   │ ** 752  676 svchost.exe 0xad8187758280  21  -   0   False   2023-05-21 22:27:43.000000  N/A \Device\HarddiskVolume3\Windows\System32\svchost
       │ .exe    C:\Windows\System32\svchost.exe -k LocalSystemNetworkRestricted -p  C:\Windows\System32\svchost.exe
  73   │ *** 3204    752 ctfmon.exe  0xad8189c8b280  12  -   1   False   2023-05-21 22:30:11.000000  N/A \Device\HarddiskVolume3\Windows\System32\ctf
       │ mon.exe "ctfmon.exe"    C:\Windows\system32\ctfmon.exe
  74   │ ** 1012 676 svchost.exe 0xad818774c080  19  -   0   False   2023-05-21 22:27:43.000000  N/A \Device\HarddiskVolume3\Windows\System32\svchost
       │ .exe    C:\Windows\System32\svchost.exe -k LocalServiceNetworkRestricted -p C:\Windows\System32\svchost.exe
  75   │ ** 1652 676 svchost.exe 0xad8187a2d2c0  10  -   0   False   2023-05-21 22:27:58.000000  N/A \Device\HarddiskVolume3\Windows\System32\svchost
       │ .exe    C:\Windows\system32\svchost.exe -k LocalServiceNetworkRestricted -p C:\Windows\system32\svchost.exe
  76   │ ** 4340 676 VSSVC.exe   0xad818e888080  3   -   0   False   2023-05-21 23:01:06.000000  N/A \Device\HarddiskVolume3\Windows\System32\VSSVC.e
       │ xe  C:\Windows\system32\vssvc.exe   C:\Windows\system32\vssvc.exe
  77   │ ** 2044 676 svchost.exe 0xad8189b27080  28  -   0   False   2023-05-21 22:49:29.000000  N/A \Device\HarddiskVolume3\Windows\System32\svchost
       │ .exe    C:\Windows\system32\svchost.exe -k wsappx -p    C:\Windows\system32\svchost.exe
  78   │ * 852   552 fontdrvhost.ex  0xad818761b0c0  5   -   0   False   2023-05-21 22:27:33.000000  N/A \Device\HarddiskVolume3\Windows\System32\fon
       │ tdrvhost.exe    -   -
  79   │ 588 520 winlogon.exe    0xad8186f450c0  5   -   1   False   2023-05-21 22:27:25.000000  N/A \Device\HarddiskVolume3\Windows\System32\winlogo
       │ n.exe   -   -
  80   │ * 1016  588 dwm.exe 0xad81876e4340  15  -   1   False   2023-05-21 22:27:38.000000  N/A \Device\HarddiskVolume3\Windows\System32\dwm.exe    
       │ "dwm.exe"   C:\Windows\system32\dwm.exe
  81   │ * 3556  588 userinit.exe    0xad818c02f340  0   -   1   False   2023-05-21 22:30:28.000000  2023-05-21 22:30:43.000000  \Device\HarddiskVolu
       │ me3\Windows\System32\userinit.exe   -   -
  82   │ ** 3580 3556    explorer.exe    0xad818c047340  76  -   1   False   2023-05-21 22:30:28.000000  N/A \Device\HarddiskVolume3\Windows\explorer
       │ .exe    C:\Windows\Explorer.EXE C:\Windows\Explorer.EXE
  83   │ *** 6724    3580    Outline.exe 0xad818e578080  0   -   1   True    2023-05-21 22:36:09.000000  2023-05-21 23:01:24.000000  \Device\Harddisk
       │ Volume3\Program Files (x86)\Outline\Outline.exe -   -
  84   │ **** 4224   6724    Outline.exe 0xad818e88b080  0   -   1   True    2023-05-21 22:36:23.000000  2023-05-21 23:01:24.000000  \Device\Harddisk
       │ Volume3\Program Files (x86)\Outline\Outline.exe -   -
  85   │ **** 4628   6724    tun2socks.exe   0xad818de82340  0   -   1   True    2023-05-21 22:40:10.000000  2023-05-21 23:01:24.000000  \Device\Hard
       │ diskVolume3\Program Files (x86)\Outline\resources\app.asar.unpacked\third_party\outline-go-tun2socks\win32\tun2socks.exe    -   -
  86   │ *** 5636    3580    notepad.exe 0xad818db45080  1   -   1   False   2023-05-21 22:46:50.000000  N/A \Device\HarddiskVolume3\Windows\System32
       │ \notepad.exe    -   -
  87   │ *** 464 3580    SecurityHealth  0xad818979d080  3   -   1   False   2023-05-21 22:31:59.000000  N/A \Device\HarddiskVolume3\Windows\System32
       │ \SecurityHealthSystray.exe  -   -
  88   │ *** 5328    3580    msedge.exe  0xad818d0980c0  54  -   1   False   2023-05-21 22:32:02.000000  N/A \Device\HarddiskVolume3\Program Files (x
       │ 86)\Microsoft\Edge\Application\msedge.exe   "C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe" --no-startup-window --win-session
       │ -start /prefetch:5  C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe
  89   │ **** 4544   5328    msedge.exe  0xad818d75b080  14  -   1   False   2023-05-21 22:32:39.000000  N/A \Device\HarddiskVolume3\Program Files (x
       │ 86)\Microsoft\Edge\Application\msedge.exe   -   -
  90   │ **** 8896   5328    msedge.exe  0xad8187a39080  18  -   1   False   2023-05-21 22:28:21.000000  N/A \Device\HarddiskVolume3\Program Files (x
       │ 86)\Microsoft\Edge\Application\msedge.exe   -   -
  91   │ **** 5156   5328    msedge.exe  0xad818c553080  14  -   1   False   2023-05-21 22:28:22.000000  N/A \Device\HarddiskVolume3\Program Files (x
       │ 86)\Microsoft\Edge\Application\msedge.exe   -   -
  92   │ **** 7964   5328    msedge.exe  0xad818dee5080  19  -   1   False   2023-05-21 22:22:09.000000  N/A \Device\HarddiskVolume3\Program Files (x
       │ 86)\Microsoft\Edge\Application\msedge.exe   -   -
  93   │ **** 4396   5328    msedge.exe  0xad818d515080  7   -   1   False   2023-05-21 22:32:19.000000  N/A \Device\HarddiskVolume3\Program Files (x
       │ 86)\Microsoft\Edge\Application\msedge.exe   -   -
  94   │ **** 6544   5328    msedge.exe  0xad818c0ea080  18  -   1   False   2023-05-21 22:22:35.000000  N/A \Device\HarddiskVolume3\Program Files (x
       │ 86)\Microsoft\Edge\Application\msedge.exe   -   -
  95   │ **** 2388   5328    msedge.exe  0xad818e54c340  18  -   1   False   2023-05-21 22:05:35.000000  N/A \Device\HarddiskVolume3\Program Files (x
       │ 86)\Microsoft\Edge\Application\msedge.exe   -   -
  96   │ **** 6292   5328    msedge.exe  0xad818d7a1080  20  -   1   False   2023-05-21 22:06:15.000000  N/A \Device\HarddiskVolume3\Program Files (x
       │ 86)\Microsoft\Edge\Application\msedge.exe   -   -
  97   │ **** 1144   5328    msedge.exe  0xad818d75f080  18  -   1   False   2023-05-21 22:32:38.000000  N/A \Device\HarddiskVolume3\Program Files (x
       │ 86)\Microsoft\Edge\Application\msedge.exe   -   -
  98   │ **** 5340   5328    msedge.exe  0xad818d7b3080  10  -   1   False   2023-05-21 22:32:39.000000  N/A \Device\HarddiskVolume3\Program Files (x
       │ 86)\Microsoft\Edge\Application\msedge.exe   -   -
  99   │ *** 3252    3580    vmtoolsd.exe    0xad8189796300  8   -   1   False   2023-05-21 22:31:59.000000  N/A \Device\HarddiskVolume3\Program File
       │ s\VMware\VMware Tools\vmtoolsd.exe  "C:\Program Files\VMware\VMware Tools\vmtoolsd.exe" -n vmusr    C:\Program Files\VMware\VMware Tools\vmt
       │ oolsd.exe
 100   │ *** 2228    3580    FTK Imager.exe  0xad818d143080  10  -   1   False   2023-05-21 22:43:56.000000  N/A \Device\HarddiskVolume3\Program File
       │ s\AccessData\FTK Imager\FTK Imager.exe  -   -
 101   │ *** 8920    3580    FTK Imager.exe  0xad818ef81080  20  -   1   False   2023-05-21 23:02:28.000000  N/A \Device\HarddiskVolume3\Program File
       │ s\AccessData\FTK Imager\FTK Imager.exe  "C:\Program Files\AccessData\FTK Imager\FTK Imager.exe"     C:\Program Files\AccessData\FTK Imager\F
       │ TK Imager.exe
 102   │ * 860   588 fontdrvhost.ex  0xad818761f140  5   -   1   False   2023-05-21 22:27:33.000000  N/A \Device\HarddiskVolume3\Windows\System32\fon
       │ tdrvhost.exe    -   -
 103   │ 5896    8844    oneetx.exe  0xad8189b41080  5   -   1   True    2023-05-21 22:30:56.000000  N/A \Device\HarddiskVolume3\Users\Tammam\AppData
       │ \Local\Temp\c3912af058\oneetx.exe   -   -
 104   │ * 7732  5896    rundll32.exe    0xad818d1912c0  1   -   1   True    2023-05-21 22:31:53.000000  N/A \Device\HarddiskVolume3\Windows\SysWOW64
       │ \rundll32.exe   -   -
───────┴─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
```

```bash
PID del proceso sospechoso: 5896 (oneetx.exe)
=============================================
PID  del proceso hijo : 7732 (rundll32.exe)
PPID del proceso hijo : 5896 (rundll32.exe)

```

#### Respuesta

`rundll32.exe`


### Q3


What is the memory protection applied to the suspicious process memory region?

¿Cuál es la protección de memoria aplicada a la región de memoria del proceso sospechoso?


### *malfind* 


```bash  
python3 vol.py -f /home/fafi/Documents/cyberdefenders/RedLine/MemoryDump.mem windows.malfind > /home/fafi/Documents/cyberdefenders/RedLine/malfindresult
```

```bash
  
───────┬─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: malfindresult
───────┼─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ Volatility 3 Framework 2.7.1
   2   │ 
   3   │ PID Process Start VPN   End VPN Tag Protection  CommitCharge    PrivateMemory   File output Notes   Hexdump Disasm
   4   │ 
   5   │ 5896    oneetx.exe  0x400000    0x437fff    VadS    PAGE_EXECUTE_READWRITE  56  1   Disabled    MZ header   
   6   │ 4d 5a 90 00 03 00 00 00 MZ......
   7   │ 04 00 00 00 ff ff 00 00 ........
   8   │ b8 00 00 00 00 00 00 00 ........
   9   │ 40 00 00 00 00 00 00 00 @.......
  10   │ 00 00 00 00 00 00 00 00 ........
  11   │ 00 00 00 00 00 00 00 00 ........
  12   │ 00 00 00 00 00 00 00 00 ........
  13   │ 00 00 00 00 00 01 00 00 ........    4d 5a 90 00 03 00 00 00 04 00 00 00 ff ff 00 00 b8 00 00 00 00 00 00 00 40 00 00 00 00 00 00 00 00 00 00
       │  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 01 00 00
  14   │ 7540    smartscreen.ex  0x2505c140000   0x2505c15ffff   VadS    PAGE_EXECUTE_READWRITE  1   1   Disabled    N/A 
  15   │ 48 89 54 24 10 48 89 4c H.T$.H.L
  16   │ 24 08 4c 89 44 24 18 4c $.L.D$.L
  17   │ 89 4c 24 20 48 8b 41 28 .L$.H.A(
  18   │ 48 8b 48 08 48 8b 51 50 H.H.H.QP
  19   │ 48 83 e2 f8 48 8b ca 48 H...H..H
  20   │ b8 60 00 14 5c 50 02 00 .`..\P..
  21   │ 00 48 2b c8 48 81 f9 70 .H+.H..p
  22   │ 0f 00 00 76 09 48 c7 c1 ...v.H..    48 89 54 24 10 48 89 4c 24 08 4c 89 44 24 18 4c 89 4c 24 20 48 8b 41 28 48 8b 48 08 48 8b 51 50 48 83 e2
       │  f8 48 8b ca 48 b8 60 00 14 5c 50 02 00 00 48 2b c8 48 81 f9 70 0f 00 00 76 09 48 c7 c1
───────┴─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
```

#### Respuesta 

`PAGE_EXECUTE_READWRITE`


### Q4

What is the name of the process responsible for the VPN connection?

¿Cuál es el nombre del proceso responsable de la conexión VPN?


##### ***Output del comando***

`python3 vol.py -f /home/fafi/Documents/cyberdefenders/RedLine/MemoryDump.mem windows.pslist`

```bash
68   │ 6724    3580    Outline.exe 0xad818e578080  0   -   1   True    2023-05-21 22:36:09.000000  2023-05-21 23:01:24.000000  Disabled
69   │ 4224    6724    Outline.exe 0xad818e88b080  0   -   1   True    2023-05-21 22:36:23.000000  2023-05-21 23:01:24.000000  Disabled
```

#### Respuesta

`Outline.exe`

### Q5

What is the attacker's IP address?

¿Cuál es la dirección IP del atacante?


```bash
python3 vol.py -f /home/fafi/Documents/cyberdefenders/RedLine/MemoryDump.mem windows.netscan > /home/fafi/Documents/cyberdefenders/RedLine/netscanresult
```


```bash
  
───────┬─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: netscanresult
───────┼─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ Volatility 3 Framework 2.7.1
   2   │ 
   3   │ Offset  Proto   LocalAddr   LocalPort   ForeignAddr ForeignPort State   PID Owner   Created
   4   │ 
   5   │ 0xad81861e2310  TCPv4   0.0.0.0 49668   0.0.0.0 0   LISTENING   1840    spoolsv.exe 2023-05-21 22:28:09.000000 
   6   │ 0xad81861e2310  TCPv6   ::  49668   ::  0   LISTENING   1840    spoolsv.exe 2023-05-21 22:28:09.000000 
   7   │ 0xad81861e2470  TCPv4   0.0.0.0 5040    0.0.0.0 0   LISTENING   1196    svchost.exe 2023-05-21 22:30:31.000000 
   8   │ 0xad81861e2730  TCPv4   0.0.0.0 135 0.0.0.0 0   LISTENING   952 svchost.exe 2023-05-21 22:27:36.000000 
   9   │ 0xad81861e2b50  TCPv4   0.0.0.0 49665   0.0.0.0 0   LISTENING   552 wininit.exe 2023-05-21 22:27:36.000000 
  10   │ 0xad81861e2b50  TCPv6   ::  49665   ::  0   LISTENING   552 wininit.exe 2023-05-21 22:27:36.000000 
  11   │ 0xad81861e2e10  TCPv4   0.0.0.0 49665   0.0.0.0 0   LISTENING   552 wininit.exe 2023-05-21 22:27:36.000000 
  12   │ 0xad81861e3230  TCPv4   0.0.0.0 49664   0.0.0.0 0   LISTENING   696 lsass.exe   2023-05-21 22:27:36.000000 
  13   │ 0xad81861e3390  TCPv4   0.0.0.0 135 0.0.0.0 0   LISTENING   952 svchost.exe 2023-05-21 22:27:36.000000 
  14   │ 0xad81861e3390  TCPv6   ::  135 ::  0   LISTENING   952 svchost.exe 2023-05-21 22:27:36.000000 
  15   │ 0xad81861e34f0  TCPv4   0.0.0.0 49664   0.0.0.0 0   LISTENING   696 lsass.exe   2023-05-21 22:27:36.000000 
  16   │ 0xad81861e34f0  TCPv6   ::  49664   ::  0   LISTENING   696 lsass.exe   2023-05-21 22:27:36.000000 
  17   │ 0xad81861e37b0  TCPv4   0.0.0.0 49666   0.0.0.0 0   LISTENING   1012    svchost.exe 2023-05-21 22:27:49.000000 
  18   │ 0xad81861e37b0  TCPv6   ::  49666   ::  0   LISTENING   1012    svchost.exe 2023-05-21 22:27:49.000000 
  19   │ 0xad81861e3910  TCPv4   0.0.0.0 49667   0.0.0.0 0   LISTENING   448 svchost.exe 2023-05-21 22:27:58.000000 
  20   │ 0xad81861e3910  TCPv6   ::  49667   ::  0   LISTENING   448 svchost.exe 2023-05-21 22:27:58.000000 
  21   │ 0xad81861e3a70  TCPv4   0.0.0.0 49668   0.0.0.0 0   LISTENING   1840    spoolsv.exe 2023-05-21 22:28:09.000000 
  22   │ 0xad81861e3bd0  TCPv4   0.0.0.0 49666   0.0.0.0 0   LISTENING   1012    svchost.exe 2023-05-21 22:27:49.000000 
  23   │ 0xad81861e3e90  TCPv4   0.0.0.0 49667   0.0.0.0 0   LISTENING   448 svchost.exe 2023-05-21 22:27:58.000000 
  24   │ 0xad818662ecb0  TCPv4   0.0.0.0 445 0.0.0.0 0   LISTENING   4   System  2023-05-21 22:29:04.000000 
  25   │ 0xad818662ecb0  TCPv6   ::  445 ::  0   LISTENING   4   System  2023-05-21 22:29:04.000000 
  26   │ 0xad818662f390  TCPv4   0.0.0.0 7680    0.0.0.0 0   LISTENING   5476    svchost.exe 2023-05-21 22:58:09.000000 
  27   │ 0xad818662f390  TCPv6   ::  7680    ::  0   LISTENING   5476    svchost.exe 2023-05-21 22:58:09.000000 
  28   │ 0xad81878518f0  UDPv4   192.168.190.141 138 *   0       4   System  2023-05-21 22:27:56.000000 
  29   │ 0xad8187852250  UDPv4   192.168.190.141 137 *   0       4   System  2023-05-21 22:27:56.000000 
  30   │ 0xad818902a5d0  TCPv4   192.168.190.141 139 0.0.0.0 0   LISTENING   4   System  2023-05-21 22:27:56.000000 
  31   │ 0xad818971f870  UDPv4   0.0.0.0 56250   *   0       6644    SkypeApp.exe    2023-05-21 22:58:07.000000 
  32   │ 0xad818971f870  UDPv6   ::  56250   *   0       6644    SkypeApp.exe    2023-05-21 22:58:07.000000 
  33   │ 0xad81897eb010  TCPv4   10.0.85.2   55439   20.22.207.36    443 CLOSED  448 svchost.exe 2023-05-21 23:00:40.000000 
  34   │ 0xad81898a6d10  UDPv4   127.0.0.1   57787   *   0       448 svchost.exe 2023-05-21 22:28:54.000000 
  35   │ 0xad81898bc7f0  UDPv4   0.0.0.0 5355    *   0       1448    svchost.exe 2023-05-21 22:57:37.000000 
  36   │ 0xad81898bc7f0  UDPv6   ::  5355    *   0       1448    svchost.exe 2023-05-21 22:57:37.000000 
  37   │ 0xad8189a291b0  TCPv4   0.0.0.0 55972   0.0.0.0 0   LISTENING   5964    svchost.exe 2023-05-21 22:27:57.000000 
  38   │ 0xad8189a291b0  TCPv6   ::  55972   ::  0   LISTENING   5964    svchost.exe 2023-05-21 22:27:57.000000 
  39   │ 0xad8189a29470  TCPv4   0.0.0.0 55972   0.0.0.0 0   LISTENING   5964    svchost.exe 2023-05-21 22:27:57.000000 
  40   │ 0xad8189a2a7b0  TCPv4   0.0.0.0 49669   0.0.0.0 0   LISTENING   676 services.exe    2023-05-21 22:29:08.000000 
  41   │ 0xad8189a2a910  TCPv4   0.0.0.0 49669   0.0.0.0 0   LISTENING   676 services.exe    2023-05-21 22:29:08.000000 
  42   │ 0xad8189a2a910  TCPv6   ::  49669   ::  0   LISTENING   676 services.exe    2023-05-21 22:29:08.000000 
  43   │ 0xad8189a30a20  TCPv4   192.168.190.141 53660   38.121.43.65    443 CLOSED  4628    tun2socks.exe   2023-05-21 22:00:25.000000 
  44   │ 0xad8189a844e0  UDPv4   10.0.85.2   58844   *   0       5328    msedge.exe  2023-05-21 22:51:53.000000 
  45   │ 0xad8189cea350  UDPv4   0.0.0.0 5050    *   0       1196    svchost.exe 2023-05-21 22:30:27.000000 
  46   │ 0xad818c17ada0  UDPv4   0.0.0.0 52051   *   0       4628    tun2socks.exe   2023-05-21 22:24:14.000000 
  47   │ 0xad818c367b30  TCPv4   192.168.190.141 49710   204.79.197.203  443 CLOSE_WAIT  1916    SearchApp.exe   2023-05-21 22:33:09.000000 
  48   │ 0xad818c3b22e0  UDPv4   0.0.0.0 63218   *   0       1448    svchost.exe 2023-05-21 22:39:15.000000 
  49   │ 0xad818c3b22e0  UDPv6   ::  63218   *   0       1448    svchost.exe 2023-05-21 22:39:15.000000 
  50   │ 0xad818d004ba0  UDPv4   0.0.0.0 63917   *   0       1448    svchost.exe 2023-05-21 23:02:48.000000 
  51   │ 0xad818d004ba0  UDPv6   ::  63917   *   0       1448    svchost.exe 2023-05-21 23:02:48.000000 
  52   │ 0xad818d1bc010  TCPv4   10.0.85.2   55424   52.182.143.208  443 CLOSE_WAIT  6644    SkypeApp.exe    2023-05-21 22:57:59.000000 
  53   │ 0xad818d2f7b00  TCPv4   10.0.85.2   55460   52.159.127.243  443 CLOSED  448 svchost.exe 2023-05-21 23:01:08.000000 
  54   │ 0xad818d5352b0  TCPv4   10.0.85.2   53659   204.79.197.237  443 CLOSED  3580    explorer.exe    2023-05-21 22:00:25.000000 
  55   │ 0xad818da19700  UDPv4   0.0.0.0 500 *   0       448 svchost.exe 2023-05-21 22:27:56.000000 
  56   │ 0xad818da1ab50  UDPv4   0.0.0.0 4500    *   0       448 svchost.exe 2023-05-21 22:27:56.000000 
  57   │ 0xad818da1d8a0  UDPv4   0.0.0.0 4500    *   0       448 svchost.exe 2023-05-21 22:27:56.000000 
  58   │ 0xad818da1d8a0  UDPv6   ::  4500    *   0       448 svchost.exe 2023-05-21 22:27:56.000000 
  59   │ 0xad818da1dbc0  UDPv4   0.0.0.0 0   *   0       448 svchost.exe 2023-05-21 22:27:57.000000 
  60   │ 0xad818da1dbc0  UDPv6   ::  0   *   0       448 svchost.exe 2023-05-21 22:27:57.000000 
  61   │ 0xad818da1e520  UDPv4   0.0.0.0 0   *   0       448 svchost.exe 2023-05-21 22:27:57.000000 
  62   │ 0xad818da1f010  UDPv4   0.0.0.0 500 *   0       448 svchost.exe 2023-05-21 22:27:56.000000 
  63   │ 0xad818da1f010  UDPv6   ::  500 *   0       448 svchost.exe 2023-05-21 22:27:56.000000 
  64   │ 0xad818da202d0  UDPv4   0.0.0.0 0   *   0       5964    svchost.exe 2023-05-21 22:27:57.000000 
  65   │ 0xad818da202d0  UDPv6   ::  0   *   0       5964    svchost.exe 2023-05-21 22:27:57.000000 
  66   │ 0xad818da21bd0  UDPv4   0.0.0.0 0   *   0       5964    svchost.exe 2023-05-21 22:27:57.000000 
  67   │ 0xad818dbc1a60  TCPv4   192.168.190.141 49713   104.119.188.96  443 CLOSE_WAIT  1916    SearchApp.exe   2023-05-21 22:33:11.000000 
  68   │ 0xad818dd05370  UDPv4   0.0.0.0 5353    *   0       5328    msedge.exe  2023-05-21 23:01:32.000000 
  69   │ 0xad818dd07440  UDPv4   0.0.0.0 5353    *   0       5328    msedge.exe  2023-05-21 23:01:32.000000 
  70   │ 0xad818dd07440  UDPv6   ::  5353    *   0       5328    msedge.exe  2023-05-21 23:01:32.000000 
  71   │ 0xad818de4aa20  TCPv4   10.0.85.2   55462   77.91.124.20    80  CLOSED  5896    oneetx.exe  2023-05-21 23:01:22.000000 
  72   │ 0xad818df1d920  TCPv4   192.168.190.141 55433   38.121.43.65    443 CLOSED  4628    tun2socks.exe   2023-05-21 23:00:02.000000 
  73   │ 0xad818e3698f0  UDPv4   0.0.0.0 5353    *   0       5328    msedge.exe  2023-05-21 22:05:24.000000 
  74   │ 0xad818e3701a0  UDPv4   0.0.0.0 5353    *   0       5328    msedge.exe  2023-05-21 22:05:24.000000 
  75   │ 0xad818e3701a0  UDPv6   ::  5353    *   0       5328    msedge.exe  2023-05-21 22:05:24.000000 
  76   │ 0xad818e370b00  UDPv4   0.0.0.0 5353    *   0       5328    msedge.exe  2023-05-21 22:05:24.000000 
  77   │ 0xad818e371dc0  UDPv4   0.0.0.0 5353    *   0       5328    msedge.exe  2023-05-21 22:05:24.000000 
  78   │ 0xad818e371dc0  UDPv6   ::  5353    *   0       5328    msedge.exe  2023-05-21 22:05:24.000000 
  79   │ 0xad818e3a1200  UDPv4   0.0.0.0 5355    *   0       1448    svchost.exe 2023-05-21 22:57:37.000000 
  80   │ 0xad818e4a6900  UDPv4   0.0.0.0 0   *   0       5480    oneetx.exe  2023-05-21 22:39:47.000000 
  81   │ 0xad818e4a6900  UDPv6   ::  0   *   0       5480    oneetx.exe  2023-05-21 22:39:47.000000 
  82   │ 0xad818e4a9650  UDPv4   0.0.0.0 0   *   0       5480    oneetx.exe  2023-05-21 22:39:47.000000 
  83   │ 0xad818e77da20  TCPv4   192.168.190.141 52434   204.79.197.200  443 CLOSED  -   -   2023-05-21 23:02:20.000000 
  84   │ 0xad818ef06c70  UDPv6   fe80::a406:8c42:43a9:413    1900    *   0       3004    svchost.exe 2023-05-21 22:40:16.000000 
  85   │ 0xad818ef09b50  UDPv6   fe80::4577:874:81a:78cd 1900    *   0       3004    svchost.exe 2023-05-21 22:40:16.000000 
  86   │ 0xad818ef0b5e0  UDPv6   ::1 1900    *   0       3004    svchost.exe 2023-05-21 22:40:16.000000 
  87   │ 0xad818ef0ec90  UDPv6   fe80::a406:8c42:43a9:413    55910   *   0       3004    svchost.exe 2023-05-21 22:40:16.000000 
  88   │ 0xad818ef0f140  UDPv6   fe80::4577:874:81a:78cd 55911   *   0       3004    svchost.exe 2023-05-21 22:40:16.000000 
  89   │ 0xad818ef0f2d0  UDPv6   ::1 55912   *   0       3004    svchost.exe 2023-05-21 22:40:16.000000 
  90   │ 0xad818ef0fdc0  UDPv4   192.168.190.141 55913   *   0       3004    svchost.exe 2023-05-21 22:40:16.000000 
  91   │ 0xad818ef10270  UDPv4   10.0.85.2   137 *   0       4   System  2023-05-21 22:40:16.000000 
  92   │ 0xad818ef11530  UDPv4   192.168.190.141 1900    *   0       3004    svchost.exe 2023-05-21 22:40:16.000000 
  93   │ 0xad818ef116c0  UDPv4   10.0.85.2   1900    *   0       3004    svchost.exe 2023-05-21 22:40:16.000000 
  94   │ 0xad818ef11850  UDPv4   10.0.85.2   138 *   0       4   System  2023-05-21 22:40:16.000000 
  95   │ 0xad818ef119e0  UDPv4   127.0.0.1   1900    *   0       3004    svchost.exe 2023-05-21 22:40:16.000000 
  96   │ 0xad818ef13150  UDPv4   10.0.85.2   55914   *   0       3004    svchost.exe 2023-05-21 22:40:16.000000 
  97   │ 0xad818ef132e0  UDPv4   127.0.0.1   55915   *   0       3004    svchost.exe 2023-05-21 22:40:16.000000 
  98   │ 0xad818ef77b40  TCPv4   192.168.190.141 55176   192.168.190.2   53  CLOSED  1448    svchost.exe 2023-05-21 23:01:39.000000 
  99   │ 0xad818f88cc80  UDPv4   0.0.0.0 5355    *   0       1448    svchost.exe 2023-05-21 23:01:26.000000 
 100   │ 0xad818f88cc80  UDPv6   ::  5355    *   0       1448    svchost.exe 2023-05-21 23:01:26.000000 
 101   │ 0xad818f894340  UDPv4   0.0.0.0 5355    *   0       1448    svchost.exe 2023-05-21 23:01:26.000000 
 102   │ 0xad8190dd8800  UDPv4   0.0.0.0 5353    *   0       1448    svchost.exe 2023-05-21 23:01:25.000000 
 103   │ 0xad8190dd8800  UDPv6   ::  5353    *   0       1448    svchost.exe 2023-05-21 23:01:25.000000 
 104   │ 0xad8190dd8990  UDPv4   0.0.0.0 5353    *   0       1448    svchost.exe 2023-05-21 23:01:25.000000 
 105   │ 0xad8190dd97a0  UDPv4   0.0.0.0 0   *   0       1448    svchost.exe 2023-05-21 23:01:25.000000 
 106   │ 0xad8190dd97a0  UDPv6   ::  0   *   0       1448    svchost.exe 2023-05-21 23:01:25.000000 
 107   │ 0xad8190e12b10  UDPv6   fe80::a406:8c42:43a9:413    1900    *   0       3004    svchost.exe 2023-05-21 23:01:29.000000 
 108   │ 0xad8190e161c0  UDPv6   ::1 1900    *   0       3004    svchost.exe 2023-05-21 23:01:29.000000 
 109   │ 0xad8190e16e40  UDPv4   192.168.190.141 1900    *   0       3004    svchost.exe 2023-05-21 23:01:29.000000 
 110   │ 0xad8190e19230  UDPv6   ::1 57094   *   0       3004    svchost.exe 2023-05-21 23:01:29.000000 
 111   │ 0xad8190e1a1d0  UDPv4   192.168.190.141 57095   *   0       3004    svchost.exe 2023-05-21 23:01:29.000000 
 112   │ 0xad8190e1a360  UDPv4   127.0.0.1   57096   *   0       3004    svchost.exe 2023-05-21 23:01:29.000000 
 113   │ 0xad8190e1a680  UDPv4   127.0.0.1   1900    *   0       3004    svchost.exe 2023-05-21 23:01:29.000000 
 114   │ 0xad8190e1acc0  UDPv6   fe80::a406:8c42:43a9:413    57093   *   0       3004    svchost.exe 2023-05-21 23:01:29.000000 
 115   │ 0xad8190e59a60  UDPv4   0.0.0.0 55536   *   0       4628    tun2socks.exe   2023-05-21 23:00:47.000000 
 116   │ 0xad8190e59d80  UDPv4   0.0.0.0 56228   *   0       4628    tun2socks.exe   2023-05-21 23:00:38.000000 
 117   │ 0xad8190e5b040  UDPv4   0.0.0.0 49734   *   0       4628    tun2socks.exe   2023-05-21 23:00:41.000000 
───────┴─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
```


```bash
          Offset       Proto   LocalAddr   LocalPort   ForeignAddr  ForeignPort  State    PID       Owner           Created
71   │ 0xad818de4aa20  TCPv4   10.0.85.2   55462       77.91.124.20     80       CLOSED   5896    oneetx.exe  2023-05-21 23:01:22.000000  
```

#### Respuesta

`77.91.124.20`


### Q6

Based on the previous artifacts. What is the name of the malware family?

Basándonos en los artefactos anteriores. ¿Cuál es el nombre de la familia de malware?

#### Subida de la IP 77.91.124.20 a Virus Total

![red line stealer](/images/labs/cyberdefenders/redline/redline2.png)

![red line stealer](/images/labs/cyberdefenders/redline/redline1.png)

#### Respuesta

`redline stealer`

### Q7

What is the full URL of the PHP file that the attacker visited?

¿Cuál es la URL completa del archivo PHP que visitó el atacante?

```bash
strings MemoryDump.mem | grep -iE "http(|s):\/\/77.91.124.20\/" 
```

#### Respuesta

**URL MODIFICADA**

`hxxp://77[.]91[.]124[.]20/store/games/index.php`

### Q8


What is the full path of the malicious executable?

¿Cuál es la ruta completa del ejecutable malicioso?

```bash
python3 vol.py -f /home/fafi/Documents/cyberdefenders/RedLine/MemoryDump.mem windows.filescan | grep "oneetx.exe"
```

#### Respuesta

`C:\Users\Tammam\AppData\Local\Temp\c3912af058\oneetx.exe`