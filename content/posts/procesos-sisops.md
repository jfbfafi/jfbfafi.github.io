+++
title = "Procesos - Sistemas Operativos"
+++



## ¿Qué es un programa?

Un programa es un **archivo** que contiene un conjunto de instrucciones compiladas a código máquina con el propósito de construir un proceso en memoria.

## ¿Qué es un proceso?

- Un proceso es la instancia de un **programa en ejecución**. 
- Es la unidad de trabajo de un sistema operativo. 
- Componentes de un proceso:
    - Conjunto de instrucciones (código).
    - Espacio de direcciones de memoria.
    - Conjunto de datos (variables, stack).
    - Estado (activo, preparado, terminado, etc).
    - Atributos que lo identifican (process id, parent process id, user id).
    - Recursos asignados (archivos, sockets, etc).
    - Uno o más hilos de Ejecución (threads).

## Imagen de proceso

![imagen de proceso](/images/posts/sisops/imgdeproceso.png) 

### El stack y el heap son de tamaño variable mientras que los otros segmentos no. Además crecen en sentido opuesto.

- El **heap** empieza al principio del espacio de direcciones y crece hacia direcciones superiores de memoria.
- El **stack** empieza al final del espacio de direcciones y crece hacia direcciones inferiores de memoria.

## PCB (Process Control Block)

Es una estructura manejada por el sistema operativo que almacena el **contexto de ejecución de un proceso** y permite identificarlo de otros procesos.

### Componentes de un PCB:
- Identificadores (PID, PPID, UID).
- Estado de los registros del procesador.
- Estado (READY,RUNNING, etc).
- Prioridad de ejecución.
- Tiempo en CPU.
- Punteros a segmentos de código, datos, heap.
- Recursos correspondientes: archivos, sockets, semáforos, etc. 