+++
title = 'Insider - Cyberdefenders (Easy)'
+++

###### Category: Endpoint Forensics

[link del laboratorio Insider](https://cyberdefenders.org/blueteam-ctf-challenges/insider/)

---

## Herramientas a utilizar

[FTK Imager](https://www.exterro.com/digital-forensics-software/ftk-imager)

---

## Unzip lab

```bash
Archivo:  64-Insider.zip
password: cyberdefenders.org
```

![Insider lab unzip](/images/labs/cyberdefenders/insider/insiderlabunzip1.png)

![Insider lab unzip 2](/images/labs/cyberdefenders/insider/insiderlabunzip2.png)



---

## Preguntas

### Q1

What distribution of Linux is being used on this machine?

¿Qué distribución de Linux se utiliza en esta máquina?


![insider lab 1](/images/labs/cyberdefenders/insider/insider1.png) 

#### Respuesta

`kali`


### Q2

What is the MD5 hash of the apache access.log?

¿Cuál es el hash MD5 del access.log del servidor apache?

#### Respuesta

`d41d8cd98f00b204e9800998ecf8427e`


### Q3

It is believed that a credential dumping tool was downloaded? What is the file name of the download?

¿Se cree que se ha descargado una herramienta de dumping de credenciales? ¿Cuál es el nombre del archivo descargado?


#### Respuesta

`mimikatz_trunk.zip`


### Q4

There was a super-secret file created. What is the absolute path?

Se ha creado un archivo supersecreto. ¿Cuál es la ruta absoluta?

![insider lab 2](/images/labs/cyberdefenders/insider/insider2.png) 


#### Respuesta

`/root/Desktop/SuperSecretFile.txt`



### Q5

What program used didyouthinkwedmakeiteasy.jpg during execution?

¿Qué programa utilizó didyouthinkwedmakeiteasy.jpg durante la ejecución?


![insider lab 3](/images/labs/cyberdefenders/insider/insider3.png) 

#### Respuesta

`binwalk`


### Q6

What is the third goal from the checklist Karen created?

¿Cuál es el tercer objetivo de la checklist que ha creado Karen?

![insider lab 4](/images/labs/cyberdefenders/insider/insider4.png) 


#### Respuesta


`Profit`



### Q7

How many times was apache run?

¿Cuántas veces se ejecutó apache?

```txt
Apache nunca se ejecutó (el archivo access.log esta vacío)
```

#### Respuesta

`0`


### Q8


It is believed this machine was used to attack another. What file proves this?

Se cree que esta máquina se utilizó para atacar a otra. ¿Qué archivo lo demuestra?

![insider lab 5](/images/labs/cyberdefenders/insider/insider5.png) 

#### Respuesta

`irZLAohL.jpeg`

### Q9

Within the Documents file path, it is believed that Karen was taunting a fellow computer expert through a bash script. Who was Karen taunting?

Dentro de la ruta del archivo Documentos, se cree que Karen se estaba burlando de un compañero experto en informática a través de un script bash. ¿De quién se burlaba Karen?

![insider lab 6](/images/labs/cyberdefenders/insider/insider6.png) 

#### Respuesta

`Young`

### Q10

A user su'd to root at 11:26 multiple times. Who was it?

Un usuario hizo root a las 11:26 varias veces. ¿Quién fue?

![insider lab 7](/images/labs/cyberdefenders/insider/insider7.png) 

#### Respuesta

`postgres`


### Q11

Based on the bash history, what is the current working directory?

Según el historial de bash, ¿cuál es el directorio de trabajo actual?

![insider lab 8](/images/labs/cyberdefenders/insider/insider8.png) 

#### Respuesta

`/root/Documents/myfirsthack/`
