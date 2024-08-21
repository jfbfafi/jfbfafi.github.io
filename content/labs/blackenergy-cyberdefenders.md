+++
title = 'BlackEnergy - Cyberdefenders (Medium)'
+++

###### Category: Endpoint Forensics

[link del laboratorio BlackEnergy](https://cyberdefenders.org/blueteam-ctf-challenges/blackenergy/)

---

## Herramientas a utilizar

[Volatility 2](https://github.com/volatilityfoundation/volatility/releases/tag/2.6.1)

[Volatility 3](https://github.com/volatilityfoundation/volatility3)

---

## Recursos 

[Volatility cheatsheet](https://blog.onfvp.com/post/volatility-cheatsheet/)

[Volatility 2 profiles](https://github.com/volatilityfoundation/volatility/wiki/2.6-Win-Profiles)

---

## Descargar laboratorio y moverlo a la máquina virtual REMnux

### unzip lab

```bash
7z x 99-BlackEnergy.zip -pcyberdefenders.org
```

```bash 
CYBERDEF-567078-20230213-171333.raw
```
---

## Preguntas

### Q1

Which volatility profile would be best for this machine?

¿Qué perfil de volatility sería el mejor para esta máquina?

&nbsp;

***windowsinfo***

```bash
volatility2 -f CYBERDEF-567078-20230213-171333.raw imageinfo > imageinforesult
```

```bash
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: imageinforesult
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │           Suggested Profile(s) : WinXPSP2x86, WinXPSP3x86 (Instantiated with WinXPSP2x86)
   2   │                      AS Layer1 : IA32PagedMemory (Kernel AS)
   3   │                      AS Layer2 : FileAddressSpace (/home/fafi/Documents/labs/BlackEnergy/CYBERDEF-56707
       │ 8-20230213-171333.raw)
   4   │                       PAE type : No PAE
   5   │                            DTB : 0x39000L
   6   │                           KDBG : 0x8054cde0L
   7   │           Number of Processors : 1
   8   │      Image Type (Service Pack) : 3
   9   │                 KPCR for CPU 0 : 0xffdff000L
  10   │              KUSER_SHARED_DATA : 0xffdf0000L
  11   │            Image date and time : 2023-02-13 18:29:11 UTC+0000
  12   │      Image local date and time : 2023-02-13 10:29:11 -0800
───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────
```

#### Respuesta

`WinXPSP2x86`


### Q2

How many processes were running when the image was acquired?

¿Cuántos procesos se estaban ejecutando cuando se adquirió la imagen?

```bash
volatility2 -f CYBERDEF-567078-20230213-171333.raw --profile=WinXPSP2x86 pslist > pslistresult
```

```bash
cat pslistresult | awk '{print $2}' | uniq > runningprocs
```

```bash
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: runningprocs
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ Name
   2   │ --------------------
   3   │ System
   4   │ smss.exe
   5   │ csrss.exe
   6   │ winlogon.exe
   7   │ services.exe
   8   │ lsass.exe
   9   │ VBoxService.exe
  10   │ svchost.exe
  11   │ explorer.exe
  12   │ spoolsv.exe
  13   │ wscntfy.exe
  14   │ alg.exe
  15   │ VBoxTray.exe
  16   │ msmsgs.exe
  17   │ taskmgr.exe
  18   │ rootkit.exe
  19   │ cmd.exe
  20   │ notepad.exe
  21   │ DumpIt.exe  
───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────
```

```bash
cat runningprocs | uniq | grep -v -e 'Name' -e '-' | wc -l
```

#### Respuesta

`19`


### Q3

What is the process ID of cmd.exe?

¿Cuál es el ID del proceso cmd.exe?


```bash
volatility2 -f CYBERDEF-567078-20230213-171333.raw --profile=WinXPSP2x86 pslist > pslistresult
```

```bash 
Offset(V)  Name                    PID   PPID   Thds     Hnds   Sess  Wow64 Start                          Exit                          
---------- -------------------- ------ ------ ------ -------- ------ ------ ------------------------------ ------------------------------

0x89a18da0 cmd.exe                1960    964      0 --------      0      0 2023-02-13 18:25:26 UTC+0000   2023-02-13 18:25:26 UTC+0000  
```

#### Respuesta

`1960`

### Q4

What is the name of the most suspicious process?

¿Cuál es el nombre del proceso más sospechoso?


```bash
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: runningprocs
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ Name
   2   │ --------------------
   3   │ System
   4   │ smss.exe
   5   │ csrss.exe
   6   │ winlogon.exe
   7   │ services.exe
   8   │ lsass.exe
   9   │ VBoxService.exe
  10   │ svchost.exe
  11   │ explorer.exe
  12   │ spoolsv.exe
  13   │ wscntfy.exe
  14   │ alg.exe
  15   │ VBoxTray.exe
  16   │ msmsgs.exe
  17   │ taskmgr.exe
  18   │**** rootkit.exe ****
  19   │ cmd.exe
  20   │ notepad.exe
  21   │ DumpIt.exe  
───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────
```

#### Respuesta

`rootkit.exe`


### Q5

Which process shows the highest likelihood of code injection?

¿Qué proceso presenta la mayor probabilidad de inyección de código?

```bash
volatility2 -f CYBERDEF-567078-20230213-171333.raw --profile=WinXPSP2x86 malfind > malfindresult
```

```bash 
cat malfindresult | grep "MZ" -B 5 -A 4
```

```bash 
Process: svchost.exe Pid: 880 Address: 0x980000
Vad Tag: VadS Protection: PAGE_EXECUTE_READWRITE
Flags: CommitCharge: 9, MemCommit: 1, PrivateMemory: 1, Protection: 6

0x00980000  4d 5a 90 00 03 00 00 00 04 00 00 00 ff ff 00 00   MZ..............
0x00980010  b8 00 00 00 00 00 00 00 40 00 00 00 00 00 00 00   ........@.......
0x00980020  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................
0x00980030  00 00 00 00 00 00 00 00 00 00 00 00 f8 00 00 00   ................
```

> La cabecera MZ es una firma presente en los archivos ejecutables (PE)


```bash
volatility2 -f CYBERDEF-567078-20230213-171333.raw --profile=WinXPSP2x86 procdump -p 880 -D dumpfiles
```

```txt
Análisis de ejecutable con herramienta capa 
```

```bash 
capa executable.880.exe
```

```bash
┍━━━━━━━━━━━━━━━━━━━━━━━━┯━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┑ 
│ md5                    │ e3703b96747f50b5eefdabe0d75ee656                                                   │
│ sha1                   │ 7e0aa05de24ab5613fc48ff248268f4700658959                                           │
│ sha256                 │ f867bd36eb94c09c18f3a3fd4d8ca2ecccb355e54d36b992644cc4f8cc51063d                   │
│ analysis               │ static                                                                             │
│ os                     │ windows                                                                            │
│ format                 │ pe                                                                                 │
│ arch                   │ i386                                                                               │
│ path                   │ /home/fafi/Documents/labs/BlackEnergy/dumpfiles/executable.880.exe                 │
┕━━━━━━━━━━━━━━━━━━━━━━━━┷━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┙

┍━━━━━━━━━━━━━━━━━━━━━━━━┯━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┑
│ ATT&CK Tactic          │ ATT&CK Technique                                                                   │
┝━━━━━━━━━━━━━━━━━━━━━━━━┿━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┥
│ DISCOVERY              │ Process Discovery T1057                                                            │
│                        │ Query Registry T1012                                                               │
│                        │ System Information Discovery T1082                                                 │
├────────────────────────┼────────────────────────────────────────────────────────────────────────────────────┤
│ EXECUTION              │ Command and Scripting Interpreter T1059                                            │
│                        │ Shared Modules T1129                                                               │
┕━━━━━━━━━━━━━━━━━━━━━━━━┷━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┙


┍━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┯━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┑
│ MBC Objective               │ MBC Behavior                                                                  │
┝━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┿━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┥
│ ANTI-BEHAVIORAL ANALYSIS    │ Conditional Execution::Runs as Service [B0025.007]                            │
├─────────────────────────────┼───────────────────────────────────────────────────────────────────────────────┤
│ DISCOVERY                   │ System Information Discovery [E1082]                                          │
├─────────────────────────────┼───────────────────────────────────────────────────────────────────────────────┤
│ EXECUTION                   │ Command and Scripting Interpreter [E1059]                                     │
├─────────────────────────────┼───────────────────────────────────────────────────────────────────────────────┤
│ OPERATING SYSTEM            │ Registry::Query Registry Value [C0036.006]                                    │
├─────────────────────────────┼───────────────────────────────────────────────────────────────────────────────┤
│ PROCESS                     │ Terminate Process [C0018]                                                     │
┕━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┷━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┙

┍━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┯━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┑
│ Capability                                           │ Namespace                                            │
┝━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┿━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┥
│ listen for remote procedure calls                    │ communication/rpc/server                             │
│ accept command line arguments                        │ host-interaction/cli                                 │
│ query environment variable                           │ host-interaction/environment-variable                │
│ get process heap flags                               │ host-interaction/process                             │
│ terminate process (2 matches)                        │ host-interaction/process/terminate                   │
│ query or enumerate registry value (4 matches)        │ host-interaction/registry                            │
│ run as service                                       │ host-interaction/service                             │
│ link function at runtime on Windows (2 matches)      │ linking/runtime-linking                              │
│ access PE header                                     │ load-code/pe                                         │
┕━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┷━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┙
```

#### Respuesta

`svchost.exe`

### Q6

There is an odd file referenced in the recent process. Provide the full path of that file.

Hay un archivo extraño al que se hace referencia en el proceso reciente. Proporcione la ruta completa de ese archivo.

> Process handles
> 
> Son identificadores únicos que permiten a los procesos interactuar con recursos del sistema. 
>
> Archivos y directorios,Claves de registro, etc.
 
```bash
volatility2 -f CYBERDEF-567078-20230213-171333.raw --profile=WinXPSP2x86 handles -p 880 -t File
```

```bash 
Offset(V)     Pid     Handle     Access Type             Details
---------- ------ ---------- ---------- ---------------- -------

0x89af0cf0    880      0x340   0x12019f File             \Device\HarddiskVolume1\WINDOWS\system32\drivers\str.sys
```

#### Respuesta

`C:\WINDOWS\system32\drivers\str.sys`


### Q7


What is the name of the injected dll file loaded from the recent process?

¿Cuál es el nombre del archivo dll inyectado que se carga desde el proceso reciente?


```bash
volatility2 -f CYBERDEF-567078-20230213-171333.raw --profile=WinXPSP2x86 ldrmodules -p 880 > ldrmodulesresult
```

```bash
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: ldrmodulesresult
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ Pid      Process              Base       InLoad InInit InMem MappedPath
   2   │ -------- -------------------- ---------- ------ ------ ----- ----------
   3   │      880 svchost.exe          0x6f880000 True   True   True  \WINDOWS\AppPatch\AcGenral.dll
   4   │      880 svchost.exe          0x01000000 True   False  True  \WINDOWS\system32\svchost.exe
   5   │      880 svchost.exe          0x77f60000 True   True   True  \WINDOWS\system32\shlwapi.dll
   6   │      880 svchost.exe          0x74f70000 True   True   True  \WINDOWS\system32\icaapi.dll
   7   │      880 svchost.exe          0x76f60000 True   True   True  \WINDOWS\system32\wldap32.dll
   8   │      880 svchost.exe          0x77c00000 True   True   True  \WINDOWS\system32\version.dll
   9   │      880 svchost.exe          0x5ad70000 True   True   True  \WINDOWS\system32\uxtheme.dll
  10   │      880 svchost.exe          0x76e80000 True   True   True  \WINDOWS\system32\rtutils.dll
  11   │      880 svchost.exe          0x771b0000 True   True   True  \WINDOWS\system32\wininet.dll
  12   │      880 svchost.exe          0x76c90000 True   True   True  \WINDOWS\system32\imagehlp.dll
  13   │      880 svchost.exe          0x76bc0000 True   True   True  \WINDOWS\system32\regapi.dll
  14   │      880 svchost.exe          0x77dd0000 True   True   True  \WINDOWS\system32\advapi32.dll
  15   │      880 svchost.exe          0x76f20000 True   True   True  \WINDOWS\system32\dnsapi.dll
  16   │      880 svchost.exe          0x77be0000 True   True   True  \WINDOWS\system32\msacm32.dll
  17   │      880 svchost.exe          0x7e1e0000 True   True   True  \WINDOWS\system32\urlmon.dll
  18   │      880 svchost.exe          0x68000000 True   True   True  \WINDOWS\system32\rsaenh.dll
  19   │      880 svchost.exe          0x722b0000 True   True   True  \WINDOWS\system32\sensapi.dll
  20   │      880 svchost.exe          0x76e10000 True   True   True  \WINDOWS\system32\adsldpc.dll
  21   │      880 svchost.exe          0x76b40000 True   True   True  \WINDOWS\system32\winmm.dll
  22   │      880 svchost.exe          0x773d0000 True   True   True  \WINDOWS\WinSxS\x86_Microsoft.Windows.Comm
       │ on-Controls_6595b64144ccf1df_6.0.2600.5512_x-ww_35d4ce83\comctl32.dll
  23   │      880 svchost.exe          0x71a50000 True   True   True  \WINDOWS\system32\mswsock.dll
  24   │      880 svchost.exe          0x5b860000 True   True   True  \WINDOWS\system32\netapi32.dll
  25   │      880 svchost.exe          0x00670000 True   True   True  \WINDOWS\system32\xpsp2res.dll
  26   │      880 svchost.exe          0x76e90000 True   True   True  \WINDOWS\system32\rasman.dll
  27   │      880 svchost.exe          0x77a80000 True   True   True  \WINDOWS\system32\crypt32.dll
  28   │      880 svchost.exe          0x71ab0000 True   True   True  \WINDOWS\system32\ws2_32.dll
  29   │      880 svchost.exe          0x77cc0000 True   True   True  \WINDOWS\system32\activeds.dll
  30   │      880 svchost.exe          0x71ad0000 True   True   True  \WINDOWS\system32\wsock32.dll
  31   │      880 svchost.exe          0x774e0000 True   True   True  \WINDOWS\system32\ole32.dll
  32   │      880 svchost.exe          0x77920000 True   True   True  \WINDOWS\system32\setupapi.dll
  33   │      880 svchost.exe          0x7e410000 True   True   True  \WINDOWS\system32\user32.dll
  34   │      880 svchost.exe          0x7c900000 True   True   True  \WINDOWS\system32\ntdll.dll
  35   │      880 svchost.exe          0x77f10000 True   True   True  \WINDOWS\system32\gdi32.dll
  36   │      880 svchost.exe          0x77120000 True   True   True  \WINDOWS\system32\oleaut32.dll
  37   │      880 svchost.exe          0x5cb70000 True   True   True  \WINDOWS\system32\shimeng.dll
  38   │      880 svchost.exe          0x74980000 True   True   True  \WINDOWS\system32\msxml3.dll
  39   │      880 svchost.exe          0x009a0000 False  False  False \WINDOWS\system32\msxml3r.dll
  40   │      880 svchost.exe          0x77e70000 True   True   True  \WINDOWS\system32\rpcrt4.dll
  41   │      880 svchost.exe          0x769c0000 True   True   True  \WINDOWS\system32\userenv.dll
  42   │      880 svchost.exe          0x7c800000 True   True   True  \WINDOWS\system32\kernel32.dll
  43   │      880 svchost.exe          0x76fd0000 True   True   True  \WINDOWS\system32\clbcatq.dll
  44   │      880 svchost.exe          0x76b20000 True   True   True  \WINDOWS\system32\atl.dll
  45   │      880 svchost.exe          0x71bf0000 True   True   True  \WINDOWS\system32\samlib.dll
  46   │      880 svchost.exe          0x77690000 True   True   True  \WINDOWS\system32\ntmarta.dll
  47   │      880 svchost.exe          0x77c10000 True   True   True  \WINDOWS\system32\msvcrt.dll
  48   │      880 svchost.exe          0x760f0000 True   True   True  \WINDOWS\system32\termsrv.dll
  49   │      880 svchost.exe          0x76fc0000 True   True   True  \WINDOWS\system32\rasadhlp.dll
  50   │      880 svchost.exe          0x76c30000 True   True   True  \WINDOWS\system32\wintrust.dll
  51   │      880 svchost.exe          0x7c9c0000 True   True   True  \WINDOWS\system32\shell32.dll
  52   │      880 svchost.exe          0x77050000 True   True   True  \WINDOWS\system32\comres.dll
  53   │      880 svchost.exe          0x76eb0000 True   True   True  \WINDOWS\system32\tapi32.dll
  54   │      880 svchost.exe          0x76a80000 True   True   True  \WINDOWS\system32\rpcss.dll
  55   │      880 svchost.exe          0x5d090000 True   True   True  \WINDOWS\system32\comctl32.dll
  56   │      880 svchost.exe          0x71aa0000 True   True   True  \WINDOWS\system32\ws2help.dll
  57   │      880 svchost.exe          0x776c0000 True   True   True  \WINDOWS\system32\authz.dll
  58   │      880 svchost.exe          0x76ee0000 True   True   True  \WINDOWS\system32\rasapi32.dll
  59   │      880 svchost.exe          0x77b20000 True   True   True  \WINDOWS\system32\msasn1.dll
  60   │      880 svchost.exe          0x75110000 True   True   True  \WINDOWS\system32\mstlsapi.dll
  61   │      880 svchost.exe          0x77fe0000 True   True   True  \WINDOWS\system32\secur32.dll
───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────
```

```bash
Pid      Process              Base       InLoad InInit InMem MappedPath
-------- -------------------- ---------- ------ ------ ----- ----------
880      svchost.exe          0x009a0000 False  False  False \WINDOWS\system32\msxml3r.dll
```

#### Respuesta

`msxml3r.dll`

### Q8

What is the base address of the injected dll?

¿Cuál es la dirección base del dll inyectado?

```bash
volatility2 -f CYBERDEF-567078-20230213-171333.raw --profile=WinXPSP2x86 malfind 
```

```bash
Process: svchost.exe Pid: 880 Address: 0x980000
```

#### Respuesta

`0x980000`
