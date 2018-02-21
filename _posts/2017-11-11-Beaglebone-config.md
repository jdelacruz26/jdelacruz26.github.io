---
layout: post
comments: true
title:  "Cómo instalar Ubuntu 16LTS en una memoria remomible en una Beaglebone Black"
desc: "Guía sobre cómo configurar un Beaglebone Black"
keywords: "Linux,Ubuntu"
date: 2017-11-11
categories: [Linux]
tags: [Linux, Ubuntu]
icon: fa-cogs
---

<script defer src="/static/js/fontawesome-all.js"></script>

Este post tiene como objetivo mostrar como instalar *Ubuntu 16LTS* en una tarjeta *Beaglebone Black* utilizando una tarjeta SD externa.

[<center>
<img src="/static/assets/img/blog/linux/beaglebone.JPG" alt="Drawing" width= "300px"/>
</center>](/static/assets/img/blog/linux/beaglebone.JPG)
<div style="text-align:center">
Beaglebone Black.
</div>

<br>
## <i class="far fa-search-plus" aria-hidden="true"></i> **Objetivos**

Los objetivos que se buscan lograr son los siguientes:

1. Instalar *Ubuntu 16LTS* en una memoria externa SD en una tarjeta *Beaglebone Black*.

1. Realizar una configuración básica de la distribución Ubuntu instalada.

<br>
## <i class="far fa-list-ul"></i> Materiales necesarios
Para llevar a cabo la configuración, se necesitan los siguientes componentes:

* Tarjeta Beaglebone Black
* Tarjeta micro SD con adaptador
* Módulo wifi via USB
* Cable de conexión USB para conectar con la computadora

<br>
## <i class="far fa-cogs" aria-hidden="true"></i> Instalación de Ubuntu 16 LTS en un tarjeta micro SD
Para poder contar con Ubuntu 16LTS ejecutándose en nuestro Beablebone Black lo primero sería descargar una imagen de la versión de dicha distribuciñon para sistemas arm:

```
$ wget https://rcn-ee.com/rootfs/2017-10-12/elinux/ubuntu-16.04.3-console-armhf-2017-10-12.tar.xz
```

este archivo puede ser verificado utilizando lo siguiente:

```
$ sha256sum ubuntu-16.04.3-console-armhf-2017-10-12.tar.xz
817cdaa038b4847862ce0715c982224ef9671d5938854e9d47d2c50b32a7e5de  ubuntu-16.04.3-console-armhf-2017-10-12.tar.xz

```

Luego procedemos a descomprimir el archivo:

```
$ tar xf ubuntu-16.04.3-console-armhf-2017-10-12.tar.xz
$ cd ubuntu-16.04.3-console-armhf-2017-10-12
```

Ua vez hecho esto podemos proceder a insertar la tarjeta micro SD en nuestro PC, para saber la ubicación de montaje de esta memoria podemos ejecutar el siguiente archivo:

```
$ sudo ./setup_sdcard.sh --probe-mmc
```
 Luego de esto, se mostrará un mensaje como el siguiente:

```
 Are you sure? I don't see [/dev/idontknow], here is what I do see...

fdisk -l:
Disk /dev/sda: 500.1 GB, 500107862016 bytes <- x86 Root Drive
Disk /dev/sdd: 3957 MB, 3957325824 bytes <- MMC/SD card

lsblk:
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda      8:0    0 465.8G  0 disk
├─sda1   8:1    0 446.9G  0 part /  <- x86 Root Partition
├─sda2   8:2    0     1K  0 part
└─sda5   8:5    0  18.9G  0 part [SWAP]
sdd      8:48   1   3.7G  0 disk
├─sdd1   8:49   1    64M  0 part
└─sdd2   8:50   1   3.6G  0 part
```

En el cual *sda* es el sistema de archivos del sistema operativo mientras que *sdd* es la memoria microSD detectada por el sistema. Es posible que en vez de la unidad *sdd* la tarjeta SD se muestre como */dev/mmcblk0*. Para quemar la imagen en la tarjeta se debe ejecutar el siguiente comando:


```
$ sudo dd if=/dev/mmcblk0 of=~/path-to-image.img bs=4096 conv=notrunc

```

luego de unos minutos la imagen debe haber sido quemada en la microSD por lo cual ahora es posible retirarla e insertarla en el *slot* correspondiente del Beaglebone Black.

Es importante mencionar que el sistema tomara unos minutos en cargar el sistema operativo, en caso de ser necesario, mantener el botón de booteo presionado desde antes del arranque para forzar al sistema a que haga el boot desde la tarjeta SD en vez desde la tarjeta de memoria interna del dispositivo.

Las credenciales por defecto para registrarse en el sistema son:

* Nombre de usuario: ubuntu
* Contraseña: temppwd

<br>
## <i class="far fa-user"></i> Personalización

Una vez hayamos terminado con éxito el proceso de instalación de la distribución de Ubuntu en nuestro Beaglebone Black podemos empezar a configurarlo. Para empezar creamos un juego nuevo de llaves ssh,

```
$ sudo rm /etc/ssh/ssh_host_*
$ sudo dpkg-reconfigure openssh-server
```

Luego podemos proceder a modificar el mensaje de bienvenida el cual incluye las credenciales por defecto del sistema,

```
$ sudo vi /etc/issue*
```
 Ahora creamos un nuevo usuario;

 ```
 $ sudo useradd --create-home --shell /bin/bash --user-group --groups sudo jorge
 $ sudo passwd jorge
 $ sudo passwd ubuntu
```

Luego instalamos algunos programas para configurar la hora actual y gestionar las redes,

```
$ sudo dpkg-reconfigure tzdata
```

y

```
$ sudo apt-get update
$ sudo apt-get install ntp
```

ahora cambiamos el nombre del sistema (host) editando los siguientes archivos:

```
$ sudo nano /etc/hostname
$ sudo nano /etc/hosts
```

y por último podemos instalar algunos paquetes de mucha a la hora de desarrollar y compilar código ya sea en *C* o *C++*,

```
$ sudo apt-get install build-essential cmake subversion graphviz doxygen libboost-all-dev libboost-dev libssl-dev

$ sudo apt-get install gcc-multilib g++-multilib
```

De esta forma podemos contar con una configuración básica de nuestra distribución Ubuntu 16LTS ejecutandose en nuestri Beaglebone Black.

<br>
## <i class="far fa-thumbs-up" aria-hidden="true"></i> **Conclusiones**
Durante esta entrada se mostró como descargar y quemar una imagen de la distribución Ubuntu 16LTS en una tarjeta microSD desde la cual se pudiera ejecutar dicha distribución, esto puede ser llevado acabo utilizando el comando **dd** junto a los argumentos que identifican el origin y destino de la imagen a grabar. Y por último se mostró como realizar una configuración básica de nuestra distribución linux ya ejecutándose en el Beaglebone Black.

---
<center>
<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Licencia Creative Commons" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br /><span xmlns:dct="http://purl.org/dc/terms/" property="dct:title">Cómo instalar Ubuntu 16LTS en una memoria remomible en una Beaglebone Black</span> por <a xmlns:cc="http://creativecommons.org/ns#" href="https://jdelacruz26.github.io" property="cc:attributionName" rel="cc:attributionURL">Jorge De La Cruz</a> se distribuye bajo una <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Licencia Creative Commons Atribución-CompartirIgual 4.0 Internacional</a>.
</center>
