+++
title = "Protocolo DNS"
+++

## ¿Qué es el protocolo DNS?

El **sistema de nombres de dominio** es un protocolo que se encarga de la traducción de **nombres de dominio** (por ejemplo: amazon.com) a **direcciones IP** entendibles por una computadora (por ejemplo: 192.0.2.44).

Este protocolo facilita la navegación por internet ya que no hace falta memorizar direcciones IP. Solo con saber el nombre del dominio al que queremos acceder se pueden solicitar los recursos asociados al mismo.

![dns diagram](/images/posts/networking/dns.png)

El cliente primero busca en su **cache** antes de solicitar al sistema operativo una nueva petición al resolver. En caso de que el cliente no conozca dónde resolver el dominio, realiza dicha petición al S.0. que a su vez revisa su cache para asegurarse de no hacer una solicitud al resolver si ya conoce la IP asociada al dominio. De lo contrario se realiza una nueva petición.

El resolver usualmente es tu **ISP (Internet Service Provider)** y como muestra el diagrama, este debe saber dónde localizar al **root name server**.

## Tipos de registros DNS (DNS records)

### registro A

Realiza un mapeo entre una dirección IPv4 y un nombre de dominio.

### registro AAAA

Realiza un mapeo entre una dirección IPv6 y un nombre de dominio.

### registro CNAME (Canonical name record)

Alias de un nombre de dominio a otro.

### registro MX (Mail exchange record)

Especifica cómo debe ser direccionado un correo electrónico en internet. Los **registros MX** apuntan a los servidores a los cuales envían un correo electrónico, y a cuál de ellos debería ser enviado en primer lugar, por prioridad.
