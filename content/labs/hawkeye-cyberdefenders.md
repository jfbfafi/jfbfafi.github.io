+++
title = 'HawkEye - Cyberdefenders (Medium)' 
+++

###### Category: Network Forensics

[link del laboratorio HawkEye](https://cyberdefenders.org/blueteam-ctf-challenges/hawkeye/)

---

## Herramientas a utilizar

[Wireshark](https://www.wireshark.org/)

[GeoIp](https://www.maxmind.com/en/geoip-demo)

[MAC Vendors](https://macvendors.com/)

[MAC Address IO](https://macaddress.io/)

[Cyberchef](https://gchq.github.io/CyberChef/)

---

## Descargar laboratorio y moverlo a la máquina virtual REMnux

### unzip lab

```bash
7z x 91-hawkeye.zip -pcyberdefenders.org
```
---

## Preguntas

### Q1

How many packets does the capture have?

¿Cuántos paquetes tiene la captura?

*Último paquete*

![last packet](/images/labs/cyberdefenders/hawkeye/hawkeye1.png)

#### Respuesta

`4003`


### Q2

At what time was the first packet captured?

¿A qué hora fue capturado el primer paquete?


![first packet date](/images/labs/cyberdefenders/hawkeye/hawkeye2.png)

#### Respuesta

`2019-04-10 20:37:07 UTC`

### Q3

What is the duration of the capture?

¿Cuál es la duración de la captura?


![duration of the capture](/images/labs/cyberdefenders/hawkeye/hawkeye3.png)

#### Respuesta

`01:03:41`


### Q4


What is the most active computer at the link level?

¿Cuál es el equipo más activo a nivel de enlace?

***Statistics → Endpoints → Ethernet***

![most active computer](/images/labs/cyberdefenders/hawkeye/hawkeye4.png)

#### Respuesta

`00:08:02:1c:47:ae`


### Q5

Manufacturer of the NIC of the most active system at the link level?

¿Fabricante del NIC del sistema más activo a nivel de enlace? 


![most active computer manufacturer](/images/labs/cyberdefenders/hawkeye/hawkeye5.png)

#### Respuesta

`Hewlett-Packard`


### Q6


Where is the headquarter of the company that manufactured the NIC of the most active computer at the link level?

¿Dónde está la sede de la empresa que fabricó el NIC del ordenador más activo a nivel de enlace?


> Hewlett Packard headquarter
>
> Hewlett Packard (HP) is headquartered in Palo Alto, California, United States. The company has 101 office locations worldwide.
>
> Palo Alto Headquarters
>
> The Palo Alto headquarters is the main office location for HP, where the company’s executive team and various departments are based.


#### Respuesta

`Palo Alto`


### Q7

The organization works with private addressing and netmask /24. How many computers in the organization are involved in the capture?

La organización trabaja con direccionamiento privado y máscara de red /24. ¿Cuántos computadores de la organización están involucrados en la captura?

***Statistics → Endpoints → IPv4***

![computers involved](/images/labs/cyberdefenders/hawkeye/hawkeye6.png)

#### Respuesta

`3`

### Q8

What is the name of the most active computer at the network level?

¿Cuál es el nombre del equipo más activo a nivel de red?


![name of the most active computer at network level](/images/labs/cyberdefenders/hawkeye/hawkeye7.png)

#### Respuesta

`BEIJING-5CD1-PC`


### Q9

What is the IP of the organization's DNS server?

¿Cuál es la IP del servidor DNS de la organización?


![DNS server](/images/labs/cyberdefenders/hawkeye/hawkeye8.png)

#### Respuesta

`10[.]4[.]10[.]4`

### Q10

What domain is the victim asking about in packet 204?

¿Por qué dominio pregunta la víctima en el paquete 204?


![domain asked](/images/labs/cyberdefenders/hawkeye/hawkeye9.png)

#### Respuesta

`proforma-invoices[.]com`

### Q11

What is the IP of the domain in the previous question?

¿Cuál es la IP del dominio de la pregunta anterior?

![IP of the domain in the previous question](/images/labs/cyberdefenders/hawkeye/hawkeye10.png)

#### Respuesta

`217[.]182[.]138[.]150`

### Q12

Indicate the country to which the IP in the previous section belongs.

Indique el país al que pertenece el IP del apartado anterior.

![Country of the IP 217[.]182[.]138[.]150](/images/labs/cyberdefenders/hawkeye/hawkeye11.png)

`France`

### Q13

What operating system does the victim's computer run?

¿Qué sistema operativo utiliza el equipo de la víctima?

![victim's OS](/images/labs/cyberdefenders/hawkeye/hawkeye12.png)

#### Respuesta

`Windows NT 6.1`

### Q14

What is the name of the malicious file downloaded by the accountant?

¿Cuál es el nombre del archivo malicioso descargado por el contador?

```txt
Paquete número 210 (presente en la captura de la pregunta anterior)
```

#### Respuesta

`tkraw_Protected99.exe`


### Q15

What is the md5 hash of the downloaded file?

¿Cuál es el hash md5 del archivo descargado?

```txt
File → Export Objects → http
        
        ↓↓↓↓↓↓↓↓
        
Guardar tkraw_Protected99.exe
```

```bash
md5sum tkraw_Protected99.exe 
```

```bash
71826ba081e303866ce2a2534491a2f7  tkraw_Protected99.exe
```

#### Respuesta

`71826ba081e303866ce2a2534491a2f7`

### Q16

What software runs the webserver that hosts the malware?

¿Qué software ejecuta el servidor web que aloja el malware?


![web server that hosts the malware](/images/labs/cyberdefenders/hawkeye/hawkeye13.png)

#### Respuesta

`LiteSpeed`


### Q17

What is the public IP of the victim's computer?

¿Cuál es la IP pública del equipo de la víctima?

![victim's public IP](/images/labs/cyberdefenders/hawkeye/hawkeye14.png)

#### Respuesta

`173[.]66[.]146[.]112`


### Q18

In which country is the email server to which the stolen information is sent?

¿En qué país se encuentra el servidor de correo al que se envía la información robada?

```txt
Aplicar filtro en wireshark => ip.addr==10.4.10.132 and smtp
```

![SMTP server country](/images/labs/cyberdefenders/hawkeye/hawkeye15.png)

#### Respuesta

`United States`


### Q19

Analyzing the first extraction of information. What software runs the email server to which the stolen data is sent?

Analizar la primera extracción de información. Qué software ejecuta el servidor de correo al que se envían los datos robados?

![SMTP server software](/images/labs/cyberdefenders/hawkeye/hawkeye16.png)

#### Respuesta

`Exim 4.91`

### Q20

To which email account is the stolen information sent?

¿A qué cuenta de correo se envía la información robada?

![email account](/images/labs/cyberdefenders/hawkeye/hawkeye17.png)

`sales.del@macwinlogistics.in`

### Q21

What is the password used by the malware to send the email?

¿Cuál es la contraseña utilizada por el malware para enviar el correo electrónico?

![password](/images/labs/cyberdefenders/hawkeye/hawkeye19.png)

![password](/images/labs/cyberdefenders/hawkeye/hawkeye18.png)

#### Respuesta

`Sales@23`

### Q22

Which malware variant exfiltrated the data?

¿Qué variante de malware exfiltró los datos?

![malware family](/images/labs/cyberdefenders/hawkeye/hawkeye20.png)

#### Respuesta

`Reborn v9`


### Q23

What are the bankofamerica access credentials? (username:password)

¿Cuáles son las credenciales de acceso a bankofamerica? (nombre de usuario:contraseña)

![username and password](/images/labs/cyberdefenders/hawkeye/hawkeye21.png)

#### Respuesta

`roman.mcguire:P@ssw0rd$`


### Q24

Every how many minutes does the collected data get exfiltrated?

¿Cada cuántos minutos se exfiltran los datos recopilados?


![data exfiltrated rate](/images/labs/cyberdefenders/hawkeye/hawkeye22.png)

***Los datos se exfiltran cada 600 segundos (10 minutos)***

#### Respuesta 

`10`
