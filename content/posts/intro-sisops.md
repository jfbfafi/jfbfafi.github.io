+++
title = "Intro Sistemas Operativos"
+++


## Kernel vs Distribución 

Un **kernel (núcleo)** es una parte de un sistema operativo donde están implementadas las funcionalidades básicas del mismo.

Una **distribución (distro)** es el kernel con aplicaciones instaladas encima de este y otras configuraciones específicas.

![kernelvsdistro](/images/posts/sisops/kernelvsdistro.png)

## ¿Qué es un sistema operativo?

Es un programa de software que cumple las siguientes funcionalidades:

- Administrador de recursos (Memoria,CPU,programas en ejecución)
- Administrador de la seguridad
- Interfaz entre computadora y usuario
- Se autoadministra

## Estructura de un sistema

![estructura](/images/posts/sisops/estructurasistema.png)

El **usuario final** solo ejecuta programas que usan recursos asignados por el sistema operativo.

El **desarrollador de aplicaciones** entiende como funciona un sistema operativo y se aprovecha de sus funcionalidades para crear programas.

El **desarrollador de sistemas operativos** entiende como funciona el hardware para el cual se crea el sistema operativo, es decir, el tipo de arquitectura y que instrucciones de assembler tiene disponibles.


## Modos de ejecución: usuario vs kernel

Existen dos modos de ejecución en un sistema operativo: **el modo de usuario y el modo kernel**. El procesador cambia entre estos modos dependiendo del tipo de código que se está ejecutando.

### Modo usuario:

- Las aplicaciones se ejecutan en este modo.
- Ejecutan instrucciones comunes (ADD,JMP,MOV).

### Modo kernel:

- Ejecutan instrucciones privilegiadas (IO,CLI)
- Los componentes principales del sistema operativo se ejecutan en este modo.
- Las tareas críticas del sistema operativo se ejecutan en modo kernel.
- Cuando un proceso en modo de usuario necesita acceder a recursos de hardware, envía una solicitud al modo kernel a través de **llamadas al sistema**.

## Llamada al sistema (syscall)

Es un mecanismo que permite a un programa en ejecución solicitar un servicio al kernel del sistema operativo. Estas llamadas al sistema son una forma de comunicación entre el programa que ejecuta en modo usuario y el kernel del sistema operativo, que se ejecuta en modo privilegiado.

### Ejemplos:

- Acceso a recursos de hardware
- Creación de programas
- Operaciones sobre archivos (crear,leer,escribir,eliminar)

### Proceso de ejecución de una syscall

![proceso de ejecución de una syscall](/images/posts/sisops/procesosyscall.png)

## Arquitectura

![arquitectura](/images/posts/sisops/arquitectura.png)
