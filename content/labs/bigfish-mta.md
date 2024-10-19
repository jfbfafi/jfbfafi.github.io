+++
title = 'BIG FISH IN A LITTLE POND - Malware-traffic-analysis'
+++

[Lab link](https://www.malware-traffic-analysis.net/2024/09/04/index.html) 


## Unzip lab 

```bash
7z x 2024-09-04-traffic-analysis-exercise.pcap.zip -pinfected_20240904
```

```bash
7z x 2024-09-04-traffic-analysis-exercise-alerts.zip -pinfected_20240904
```

```bash
MTA/BIG_FISH_IN_A_LITTLE_POND/2024-09-04-traffic-analysis-exercise-alerts 
❯ ls
2024-09-04-traffic-analysis-exercise-alerts.jpg  2024-09-04-traffic-analysis-exercise-alerts.txt
```

---

## Incident report


## Executive Summary

What happened (when, who, what).

Qué ocurrió (cuándo, quién, qué).

Se generó un envento en el que un equipo fue infectado con el malware Koi Stealer el dia y horario **2024-09-04 17:35 UTC**.

## Victim Details 

```txt 
Hostname: DESKTOP-RNVO9AT
IP: 172[.]17[.]0[.]99 
MAC address: 18:3d:a2:b6:8d:c4
Windows user account name: afletcher
Victim's name: Andrew Fletcher
```
![Victim's name](/images/labs/mta/bigfish/name.png)

## Indicators of Compromise (IOCs)

### Alertas

![infected machine](/images/labs/mta/bigfish/alert.png)

![infected machine](/images/labs/mta/bigfish/alert2.png)

### Virus Total

![Virus Total output](/images/labs/mta/bigfish/virustotal.png)

### URLs 

![URLs](/images/labs/mta/bigfish/urls.png)


