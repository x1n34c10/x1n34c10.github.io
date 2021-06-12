---
title: "HackTheBox - ScriptKiddie"
layout: single
excerpt: Para el articulo de hoy, tengo pensado enseñar en que consiste una inyección de código malicioso HTML, o mas conocida como “HTML injection”. Antes de iniciar con la explotación de esta vulnerabilidad, que esta tan presente en los servidores web, voy a hacer un corto resumen de ¿Qué es HTML? y ¿Para que sirve?.
header:
show_date: true
classes: wide
header:
  teaser: "https://user-images.githubusercontent.com/69093629/121774633-966fdd80-cb83-11eb-8487-b04899f020a1.png"
  teaser_home_page: true
  icon: "https://user-images.githubusercontent.com/69093629/121774633-966fdd80-cb83-11eb-8487-b04899f020a1.png"
categories:
  - HackTheBox
tags:
  - WriteUp
---

<p align="center">
<img src="https://user-images.githubusercontent.com/69093629/121781072-6c2e1800-cba3-11eb-8049-597f54a08b1c.jpg">
</p>

![Captura de pantalla (500)](https://user-images.githubusercontent.com/69093629/121773974-bef5d880-cb7f-11eb-9991-ffa9f7add5c5.png)
![Captura de pantalla (498)](https://user-images.githubusercontent.com/69093629/121773472-7983dc00-cb7c-11eb-8622-6b99a8ea4679.png)

Empece haciendo un escaneo con Nmap para ver que puertos y servicios tenía corriendo el servidor.

```bash
┌─[root@parrot]─[/home/wackyhacker/Desktop]
└──╼ nmap -sS --min-rate=5000 -p- -v -Pn -n 10.10.10.226 -oG allports

Starting Nmap 7.80 ( https://nmap.org ) at 2021-06-12 11:26 CEST
Initiating SYN Stealth Scan at 11:26
Scanning 10.10.10.226 [65535 ports]
Discovered open port 22/tcp on 10.10.10.226
Discovered open port 5000/tcp on 10.10.10.226
Completed SYN Stealth Scan at 11:27, 13.04s elapsed (65535 total ports)
Nmap scan report for 10.10.10.226
Host is up (0.13s latency).
Not shown: 65533 closed ports
PORT     STATE SERVICE
22/tcp   open  ssh
5000/tcp open  upnp

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 13.12 seconds
           Raw packets sent: 65641 (2.888MB) | Rcvd: 65543 (2.622MB)
``` 

Una vez finalizado el escaneo, hice otro escaneo para saber la versión que corría de los puertos 22 y 5000.

```bash
┌─[root@parrot]─[/home/wackyhacker/Desktop]
└──╼ nmap -sC -sV -p22,5000 10.10.10.226 -oN targeted      
 
Starting Nmap 7.80 ( https://nmap.org ) at 2021-06-12 11:28 CEST
Nmap scan report for 10.10.10.226
Host is up (0.042s latency).

PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.1 (Ubuntu Linux; protocol 2.0)
5000/tcp open  http    Werkzeug httpd 0.16.1 (Python 3.8.5)
|_http-title: k1d'5 h4ck3r t00l5
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 10.86 seconds
```

El puerto 22 era SSH de versión 8.2p1 y estaba corriendo en un sistema Ubuntu, y el puerto 5000 es un servidor web, empece enumerando el servidor web para ver que traía, este fue el resultado.

![image](https://user-images.githubusercontent.com/69093629/121775004-59a4e600-cb85-11eb-9afe-09c9d5ac8f02.png)




