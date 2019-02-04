---
layout: post
title: "Plasma Mobile en Raspberry Pi 3"
date: "2019-02-04 12:06:33 +0100"
author: Julio Molina Soler <julio@molinasoler.xyz>
tags: RaspberryPi
---
## Indice

Que vamos a cubrir en este articulo?
0. Hardware usado
1. Actualizar Raspbian a Version Buster
2. Instalar Plasma Mobile
3. Usar Plasma Mobile por Defecto

## Hardware usado

## Actualizar Raspbian a Version Buster
Primero comprueba que version de raspbian tu Raspberry Pi tiene instalada.
Para ello accedemos al terminal y ejecutamos el comando `lsb_release -a`
```
$ lsb_release -a
No LSB modules are available.
Distributor ID:	Raspbian
Description:	Raspbian GNU/Linux 9.6 (stretch)
Release:	9.6
Codename:	stretch
```
A fecha de Febrero 2019, la version Buster se encuentra en fase de testeo. Para poder acceder a ella necesitamos actualizar la version de raspbian Stretch a raspbian Buster, el procedimiento es el mismo que hariamos en cualquier version de Debian.

Primero necesito que no haya ninguna actualizacion pendiente.
```
$ sudo apt update
$ sudo apt upgrade -y
$ sudo apt dist-upgrade
```
Al final reinicio para evitar sorpresas durante el proceso de actualizacion a la nueva version.

```
$ sudo reboot
```

Siguiente paso, comento toda referencia a los repositorios de stretch. Todos los datos se encuentran en `/etc/apt/sources.list.d/` o en el fichero `/etc/apt/sources.list` (mejor evitar tener nada en ese fichero, para si poder gestionar los repositorios de forma mas modular).

### Cambio en fichero raspi.list
```
### Antes ###
pi@portos:~ $ sudo cat /etc/apt/sources.list.d/raspi.list
deb http://archive.raspberrypi.org/debian/ stretch main ui
# Uncomment line below then 'apt-get update' to enable 'apt-get source'
#deb-src http://archive.raspberrypi.org/debian/ stretch main ui
--
### Después ###
pi@portos:~ $ sudo cat /etc/apt/sources.list.d/raspi.list
#deb http://archive.raspberrypi.org/debian/ stretch main ui
# Uncomment line below then 'apt-get update' to enable 'apt-get source'
#deb-src http://archive.raspberrypi.org/debian/ stretch main ui
```

### Crear fichero raspi_buster.list
Copio el fichero `/etc/apt/sources.list.d/raspi.list` a `/etc/apt/sources.list.d/raspi_buster.list` y cambio stretch a buster y anado varias palabras para tener acceso a los extras.

```
pi@portos:~ $ sudo cat /etc/apt/sources.list.d/raspi_buster.list
deb http://archive.raspberrypi.org/debian/ buster main ui contrib non-free rpi
# Uncomment line below then 'apt-get update' to enable 'apt-get source'
#deb-src http://archive.raspberrypi.org/debian/ stretch main ui

pi@portos:~ $ sudo apt update
Hit:1 http://archive.raspberrypi.org/debian buster InRelease
Reading package lists... Done
Building dependency tree
Reading state information... Done
All packages are up to date.
```
Empiezo con la actualizacion a Buster.
```
pi@portos:~ $ sudo apt upgrade -y
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.

pi@portos:~ $ sudo apt dist-upgrade -y
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.

pi@portos:~ $ sudo rpi-update
 *** Raspberry Pi firmware updater by Hexxeh, enhanced by AndrewS and Dom
 *** Performing self-update
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 13545  100 13545    0     0  23385      0 --:--:-- --:--:-- --:--:-- 23393
 *** Relaunching after update
 *** Raspberry Pi firmware updater by Hexxeh, enhanced by AndrewS and Dom
 *** We're running for the first time
 *** Backing up files (this will take a few minutes)
 *** Backing up firmware
 *** Backing up modules 4.14.79-v7+
#############################################################
This update bumps to rpi-4.14.y linux tree
Be aware there could be compatibility issues with some drivers
Discussion here:
https://www.raspberrypi.org/forums/viewtopic.php?f=29&t=197689
##############################################################
 *** Downloading specific firmware revision (this will take a few minutes)
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   168    0   168    0     0    329      0 --:--:-- --:--:-- --:--:--   328
100 56.2M  100 56.2M    0     0   503k      0  0:01:54  0:01:54 --:--:--  543k
 *** Updating firmware
 *** Updating kernel modules
 *** depmod 4.14.94-v7+
 *** depmod 4.14.94+
 *** Updating VideoCore libraries
 *** Using HardFP libraries
 *** Updating SDK
 *** Running ldconfig
 *** Storing current firmware revision
 *** Deleting downloaded files
 *** Syncing changes to disk
 *** If no errors appeared, your firmware was successfully updated to 699879be36d90225232de87e9ae589be7209b14c
 *** A reboot is needed to activate the new firmware

 pi@portos:~ $ sudo reboot
```
