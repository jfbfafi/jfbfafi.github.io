+++
title = 'Sysinternals - Cyberdefenders (Medium)'
+++

###### Category: Endpoint Forensics

[link del laboratorio Sysinternals](https://cyberdefenders.org/blueteam-ctf-challenges/sysinternals/)

---

## Herramientas a utilizar

[FTK Imager](https://www.exterro.com/digital-forensics-software/ftk-imager)


[Eric Zimmerman's tools](https://ericzimmerman.github.io/#!index.md) -> AppCompatCacheParser, AmcacheParser

---

## Preguntas

### Q1

What was the malicious executable file name that the user downloaded?

¿Cuál era el nombre del archivo ejecutable malicioso que descargó el usuario?

![Sysinternals 1](/images/labs/cyberdefenders/sysinternals/sysinternals1.png) 

#### Respuesta

`Sysinternals.exe`


### Q2


When was the last time the malicious executable file was modified? 12-hour format

¿Cuándo fue la última vez que se modificó el archivo ejecutable malicioso? Formato de 12 horas

```powershell
PS C:\Users\winmalwareuser\tools> .\AppCompatCacheParser.exe -f C:\Users\winmalwareuser\labs\100-sysinternalslab\config\SYSTEM --csv C:\Users\winmalwareuser\labs\100-sysinternalslab\exported
```


```bash
=======================
Registro en archivo CSV
=======================

C:\Users\Public\Downloads\SysInternals.exe	2022-11-15 21:18:51
```

#### Respuesta (*Formato de 12 horas*)

`11/15/2022 09:18:51 PM`


### Q3

What is the SHA1 hash value of the malware?

¿Cuál es el valor hash SHA1 del malware?


```powershell
PS C:\Users\winmalwareuser\tools\AmcacheParser> .\AmcacheParser.exe -f C:\Users\winmalwareuser\labs\100-sysinternalslab\exported\Amcache.hve --csv C:\Users\winmalwareuser\labs\100-sysinternalslab\exported
AmcacheParser version 1.5.1.0

Author: Eric Zimmerman (saericzimmerman@gmail.com)
https://github.com/EricZimmerman/AmcacheParser

Command line: -f C:\Users\winmalwareuser\labs\100-sysinternalslab\exported\Amcache.hve --csv C:\Users\winmalwareuser\labs\100-sysinternalslab\exported

Warning: Administrator privileges not found!


C:\Users\winmalwareuser\labs\100-sysinternalslab\exported\Amcache.hve is in new format!

Total file entries found: 36
Total device containers found: 4
Total device PnPs found: 83

Found 36 unassociated file entry

Results saved to: C:\Users\winmalwareuser\labs\100-sysinternalslab\exported

Total parsing time: 0,257 seconds

```

#### Respuesta

`fa1002b02fc5551e075ec44bb4ff9cc13d563dcf`

### Q4

What is the malware's family?

¿Cuál es la familia del malware?

---

```txt
Hash de la respuesta anterior subido a Virus Total
```

![Sysinternals 2](/images/labs/cyberdefenders/sysinternals/sysinternals2.png) 

---

#### Respuesta

`Rozena`

### Q5

What is the first mapped domain's Fully Qualified Domain Name (FQDN)?

¿Cuál es el nombre de dominio completo (FQDN) del primer dominio asignado?

---

```txt
======
HINT 1
======

Check files under Users\IEUser\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine

```

![Sysinternals 3](/images/labs/cyberdefenders/sysinternals/sysinternals3.png) 

---

#### Respuesta

```txt
=================================================
Remover los corchetes para completar la respuesta
=================================================
```

`www[.]malware430[.]com`

### Q6

The mapped domain is linked to an IP address. What is that IP address?

El dominio mapeado está vinculado a una dirección IP. Cuál es esa dirección IP?

```txt
Respuesta presente en la captura de la pregunta anterior 
```

#### Respuesta

```txt
=================================================
Remover los corchetes para completar la respuesta
=================================================
```

`192[.]168[.]15[.]10`


### Q7

What is the name of the executable dropped by the first-stage executable?

¿Cuál es el nombre del ejecutable descargado por el ejecutable de la primera etapa?


```txt
==================================
- Archivo SysInternals.exe corrupto 
- No se pueden conseguir los strings del ejecutable
==================================

==================================
Copia del archivo SysInternals.exe en -> Users/IEUser/AppData/Local/Packages/Microsoft.MicrosoftEdge_8wekyb3d8bbwe/AC/#!001/MicrosoftEdge/Cache/WMFWC1O7/SysInternals[1].exe
```

---

```txt
RESULTADOS DE LA HERRAMIENTA => FLOSS   
```

```txt
PS C:\Users\winmalwareuser\labs\100-sysinternalslab\exported> C:\Users\winmalwareuser\tools\floss-v3.1.0-windows\floss.exe .\SysInternals1.exe
```

```txt
 FLARE FLOSS RESULTS (version v3.1.0-0-gdb9af41)

+------------------------+------------------------------------------------------------------------------------+
| file path              | SysInternals[1].exe                                                                |
| identified language    | unknown                                                                            |
| extracted strings      |                                                                                    |
|  static strings        | 238 (2889 characters)                                                              |
|   language strings     |   0 (   0 characters)                                                              |
|  stack strings         | 1                                                                                  |
|  tight strings         | 0                                                                                  |
|  decoded strings       | 7                                                                                  |
+------------------------+------------------------------------------------------------------------------------+


 ────────────────────────────
  FLOSS STATIC STRINGS (238)
 ────────────────────────────

+-----------------------------------+
| FLOSS STATIC STRINGS: ASCII (217) |
+-----------------------------------+

!This program cannot be run in DOS mode.
Rich(
.text
`.rdata
@.data
.rsrc
@.reloc
QSVW
40A@;
Y_^[
h !@
==3@
u"h@3@
hL3@
h8&@
Y_^[
=<3@
=@3@
h@3@
hX3@
>csm
%p3@
Y__^[
%t3@
SVW3
ntel
5ineI
5Genu
t#=`
=x3@
=x3@
=x3@
=x3@
5t3@
_^[3
%P @
%D @
%L @
%H @
%x @
%p @
%h @
%  @
=t3@
)551{nn666o&..&-$o".,
)5512{nn%."2o,("3.2.'5o".,n$/l42n282(/5$3/ -2n
)551{nn666o, -6 3$urqo".,n)5,-n
6 3$
1% 5$o$9$
)5512{nn%.6/-. %o282(/5$3/ -2o".,n'(-$2n\t$9s
$"o;(1
IE Agent 11.0
c:\Windows\vmtoolsIO.exe
c:\Windows\
/C c:\Windows\vmtoolsIO.exe -install && net start VMwareIOHelperService && sc config VMwareIOHelperService start= auto
cmd.exe
open
c:\Windows\Temp\Hex2Dec.zip
AAAAAAAAAAAAAAAA
GCTL
.text$mn
.idata$5
.00cfg
.CRT$XCA
.CRT$XCAA
.CRT$XCZ
.CRT$XIA
.CRT$XIAA
.CRT$XIAC
.CRT$XIZ
.CRT$XPA
.CRT$XPZ
.CRT$XTA
.CRT$XTZ
.rdata
.rdata$sxdata
.rdata$zzzdbg
.rtc$IAA
.rtc$IZZ
.rtc$TAA
.rtc$TZZ
.xdata$x
.idata$2
.idata$3
.idata$4
.idata$6
.data
.bss
.rsrc$01
.rsrc$02
Sleep
FreeConsole
KERNEL32.dll
ShellExecuteA
SHELL32.dll
InternetOpenUrlA
InternetOpenA
InternetCloseHandle
WININET.dll
URLDownloadToFileA
urlmon.dll
__current_exception
__current_exception_context
memset
_except_handler4_common
VCRUNTIME140.dll
_seh_filter_exe
_set_app_type
__setusermatherr
_configure_narrow_argv
_initialize_narrow_environment
_get_initial_narrow_environment
_initterm
_initterm_e
exit
_exit
_set_fmode
__p___argc
__p___argv
_cexit
_c_exit
_register_thread_local_exe_atexit_callback
_configthreadlocale
_set_new_mode
__p__commode
_initialize_onexit_table
_register_onexit_function
_crt_atexit
_controlfp_s
terminate
api-ms-win-crt-runtime-l1-1-0.dll
api-ms-win-crt-math-l1-1-0.dll
api-ms-win-crt-stdio-l1-1-0.dll
api-ms-win-crt-locale-l1-1-0.dll
api-ms-win-crt-heap-l1-1-0.dll
UnhandledExceptionFilter
SetUnhandledExceptionFilter
GetCurrentProcess
TerminateProcess
IsProcessorFeaturePresent
QueryPerformanceCounter
GetCurrentProcessId
GetCurrentThreadId
GetSystemTimeAsFileTime
InitializeSListHead
IsDebuggerPresent
GetModuleHandleW
IHDR
sRGB
gAMA
\tpHYs
IDATx^
sgNlnn&
EvvvN
j:88
ID@75
_YYI
+++y
ZV#@
 ,"@
Ky\t8B
aN@l
aN@l
Ji8$`
`ff&
baa!MLL
R^+G
(Kv$7n
\t@7b
IEND
\t\t\t\t\t\t\t\t
\t\t\t\t
\t\t\t\t\t
\t\t\t\t\t\t\t\t
\t\t\t\t\t
\t\t\t\t\t
IHDR
sRGB
gAMA
\tpHYs
IDATx^
'n[vA
B@/M
_?B@
fVWW
P$B`
Omii
:fqH@
Z~/@
p^]S
bKKK
\tp<i
jn"P
:j}p
IEND
<?xml version='1.0' encoding='UTF-8' standalone='yes'?>
<assembly xmlns='urn:schemas-microsoft-com:asm.v1' manifestVersion='1.0'>
  <trustInfo xmlns="urn:schemas-microsoft-com:asm.v3">
    <security>
      <requestedPrivileges>
        <requestedExecutionLevel level='asInvoker' uiAccess='false' />
      </requestedPrivileges>
    </security>
  </trustInfo>
</assembly>
0!0(0/050:0A0J0O0V0]0g0o0v0|0
1%1s1
2-2t2
3/3D3I3N3o3t3
5&5.565B5K5P5V5`5j5z5
6/6b6
8"8+888N8
:9:y:
;);A;^;
===F=O=]=f=
> >&>,>2>8>>>D>J>P>Z>
0 1$1D3H3P3,606L6P6


+-------------------------------------+
| FLOSS STATIC STRINGS: UTF-16LE (21) |
+-------------------------------------+

VS_VERSION_INFO
StringFileInfo
040904b0
CompanyName
SysInternals, Inc.
FileDescription
SysInternals Suite Downloader
FileVersion
2.0.0.1
InternalName
SysInternals.exe
LegalCopyright
Copyright (C) 2020
OriginalFilename
SysInternals.exe
ProductName
SysInternals Suite Downloader
ProductVersion
2.0.0.1
VarFileInfo
Translation


 ─────────────────────────
  FLOSS STACK STRINGS (1)
 ─────────────────────────

ineIGenu


 ─────────────────────────
  FLOSS TIGHT STRINGS (0)
 ─────────────────────────



 ───────────────────────────
  FLOSS DECODED STRINGS (7)
 ───────────────────────────

)551{nn666o, -6 3$urqo".,n)5,-n
1% 5$o$9$
)5512{nn%.6/-. %o282(/5$3/ -2o".,n'(-$2n\t$9s
hxxp://www.google.com
hxxps://docs[.]microsoft[.]com/en-us/sysinternals/
hxxp://www[.]malware430[.]com/html/VMwareUpdate.exe
hxxps://download[.]sysinternals[.]com/files/Hex2Dec.zip 
```
---

#### Respuesta

`vmtoolsIO.exe`


### Q8

What is the name of the service installed by 2nd stage executable?

¿Cuál es el nombre del servicio instalado por el ejecutable de 2ª etapa?

```txt
En el output de la herramienta floss se encuentra el comando que ejecuta vmtoolsIO.exe   

===========
COMANDO ↓↓↓
===========

c:\Windows\vmtoolsIO.exe -install && net start VMwareIOHelperService && sc config VMwareIOHelperService start= auto
```

#### Respuesta

`VMwareIOHelperService`

### Q9

What is the extension of files deleted by the 2nd stage executable?

¿Cuál es la extensión de los archivos eliminados por el ejecutable de la 2ª fase?

```txt
Exportar ejecutable y extraer strings con floss => c:\Windows\vmtoolsIO.exe 
```

```powershell
PS C:\Users\winmalwareuser\labs\100-sysinternalslab\exported> C:\Users\winmalwareuser\tools\floss-v3.1.0-windows\floss.exe .\vmtoolsIO.exe 
```

```txt
========================================
PARTE DEL OUTPUT DE LA HERRAMIENTA FLOSS
========================================

NT AUTHORITY\SYSTEM
VMWare IO Helper Service
VMwareIOHelperService
remove
Parameters:
 -install  to install the service.
 -remove   to remove the service.
Service failed to run w/err 0x%08lx
VMwareIOHelperService in OnStart
*.pf
C:\Windows\Prefetch
VMwareIOHelperService in OnStop
@Service Start
Service failed to start.
Service Stop
Service failed to stop.
Service Pause
Service failed to pause.
Service Continue
Service failed to resume.
Service Shutdown
Service failed to shut down.
%s failed w/err 0x%08lx
GetModuleFileName failed w/err 0x%08lx
OpenSCManager failed w/err 0x%08lx
CreateService failed w/err 0x%08lx
%s is installed.
OpenService failed w/err 0x%08lx
Stopping %s.
%s is stopped.
%s failed to stop.
DeleteService failed w/err 0x%08lx
%s is removed.
VS_VERSION_INFO
StringFileInfo
040904b0
CompanyName
VMware, Inc.
FileDescription
VMware Input & Output Helper Service
FileVersion
3.2.0.1
InternalName
vmtoolsIO.exe
LegalCopyright
Copyright (C) 2020
OriginalFilename
vmtoolsIO.exe
ProductName
VMware Input & Output Helper Service
ProductVersion
3.2.0.1
VarFileInfo
Translation


 ─────────────────────────
  FLOSS STACK STRINGS (2)
 ─────────────────────────

\Windows\Prefetch
ineIGenu


 ─────────────────────────
  FLOSS TIGHT STRINGS (0)
 ─────────────────────────



 ───────────────────────────
  FLOSS DECODED STRINGS (3)
 ───────────────────────────

C:\Windows\Prefetch\*.*
C:\Windows\Prefetch\
C:\Windows\Prefetch\*.pf  
```

#### Respuesta 

`pf`
