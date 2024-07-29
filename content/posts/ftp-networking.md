+++
title = 'Protocolo FTP'
+++

## ¿Qué es el protocolo FTP?

**FTP** son las siglas de **File Transfer Protocol (Protocolo de Transferencia de Archivos)**, que es un protocolo de red estándar destinado a transferir archivos entre un ordenador local y un servidor remoto a través de Internet. Se trata de un protocolo cliente-servidor, en el que el cliente inicia la conexión con el servidor y éste responde con una lista de archivos y directorios disponibles.

## Configuración en linux

### Instalación

```bash
sudo apt install vsftpd ftp ufw
```

### Status del servicio vsftpd

```bash
systemctl status vsftpd
```

```bash
● vsftpd.service - vsftpd FTP server
     Loaded: loaded (/lib/systemd/system/vsftpd.service; enabled; preset: enabled)
     Active: active (running) since Mon 2024-06-03 10:19:03 EDT; 18min ago
   Main PID: 2612 (vsftpd)
      Tasks: 1 (limit: 4623)
     Memory: 876.0K
        CPU: 5ms
     CGroup: /system.slice/vsftpd.service
             └─2612 /usr/sbin/vsftpd /etc/vsftpd.conf
```

#### Iniciar en caso de que no este activo

```bash
sudo systemctl start vsftpd
```

## Crear usuario ftp

```bash
sudo useradd -m ftp_client
sudo passwd ftp_client 
```

## Crear archivo de prueba 

```bash
su ftp_client
python3 -c 'import pty;pty.spawn("/bin/bash")'
echo "test file" > test.txt
```

## Login como ftp_client

```bash
ftp localhost
Trying [::1]:21 ...
Connected to localhost.
220 (vsFTPd 3.0.3)
Name (localhost:fafi): ftp_client
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> dir
229 Entering Extended Passive Mode (|||57616|)
150 Here comes the directory listing.
-rw-r--r--    1 1001     1001           10 Jun 03 11:09 test.txt
226 Directory send OK.
ftp> exit
221 Goodbye.
```

## Reiniciar servicio 

```bash
sudo systemctl restart vsftpd
```


## Archivo de configuración

```bash
sudo nano /etc/vsftpd.conf 
```

## Backup de archivo de configuración 
### Realizar antes de hacer cualquier cambio

```bash
sudo cp /etc/vsftpd.conf /etc/vsftpd.conf.bak 
```

## Descomentar write_enable para subir archivos

```bash
# Uncomment this to enable any form of FTP write command.
#write_enable=YES

write_enable=YES
```
### Reiniciar servicio después de cada modificación del archivo de configuración

```bash
sudo systemctl restart vsftpd
```

## Subir archivo test_upload.txt y descargar archivo test.txt
 
```bash
echo "test upload" > test_upload.txt
fafi@debian ~/Documents> ftp localhost
Trying [::1]:21 ...
Connected to localhost.
220 (vsFTPd 3.0.3)
Name (localhost:fafi): ftp_client
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> put test_upload.txt
local: test_upload.txt remote: test_upload.txt
229 Entering Extended Passive Mode (|||62127|)
150 Ok to send data.
100% |*****************************************************************************|    12      213.06 KiB/s    00:00 ETA
226 Transfer complete.
12 bytes sent in 00:00 (28.10 KiB/s)
ftp> dir
229 Entering Extended Passive Mode (|||45753|)
150 Here comes the directory listing.
-rw-r--r--    1 1001     1001           10 Jun 03 11:09 test.txt
-rw-------    1 1001     1001           12 Jun 03 11:54 test_upload.txt
226 Directory send OK.
ftp> get test.txt
local: test.txt remote: test.txt
229 Entering Extended Passive Mode (|||41988|)
150 Opening BINARY mode data connection for test.txt (10 bytes).
100% |*****************************************************************************|    10      152.58 KiB/s    00:00 ETA
226 Transfer complete.
10 bytes received in 00:00 (32.66 KiB/s)
ftp> exit
221 Goodbye.
```

```bash
fafi@debian ~/Documents> ls
test.txt  test_upload.txt
```

## Hablitar acceso externo

```bash
fafi@debian ~ > sudo ufw allow ssh

fafi@debian ~ > sudo ufw allow from any to any port 20,21 proto tcp

fafi@debian ~ > sudo ufw enable 
```
