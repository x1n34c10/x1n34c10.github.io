---
title: "HackTheBox - ScriptKiddie"
layout: single
excerpt: Este es el "Write-Up" de la máquina **ScriptKiddie** de la plataforma HackTheBox, una máquina bastante interesante y creativa por parte del creador...
header:
show_date: true
classes: wide
header:
  teaser: "https://user-images.githubusercontent.com/69093629/121801513-f6c05700-cc37-11eb-8588-be69ce952ec7.jpg"
  teaser_home_page: true
  icon: "https://user-images.githubusercontent.com/69093629/121801513-f6c05700-cc37-11eb-8588-be69ce952ec7.jpg"
categories:
  - HackTheBox
tags:
  - WriteUp
---

<p align="center">
<img src="https://user-images.githubusercontent.com/69093629/121801513-f6c05700-cc37-11eb-8588-be69ce952ec7.jpg">
</p>

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

| NAME  | ScriptKiddie    | 
| ------------- |:-------------:| 
| MACHINE RATING     | ★★★★   |
| USER OWNS     | 13689 👤      |   
| SYSTEM OWNS | 9787 #          |  
|  RELEASE DATE |  125 Days 🗓  |
| MACHINE CREATOR | 0xdf |

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

Comencé a probar inyecciones de comandos en los buffers que habían, pero nada interesante.

![Captura de pantalla (504)](https://user-images.githubusercontent.com/69093629/121785032-b9b48000-cbb7-11eb-8f0e-93e43ebdad7f.png)

Hasta que me decante por el buffer del medio que permitía subir un archivo, ponía *template file (optional)*, busque *template apk* en searchsploit y encontré el siguiente "exploit" hecho en Metasploit.

![Captura de pantalla (505)](https://user-images.githubusercontent.com/69093629/121785124-40695d00-cbb8-11eb-85b2-e1cafd29153c.png)

Examine el "exploit".

![Captura de pantalla (506)](https://user-images.githubusercontent.com/69093629/121785156-74dd1900-cbb8-11eb-82f1-04c1de91f090.png)

Una vez encontré el *CVE*, me dirigí a Google y busqué algún "exploit" que esté en GitHub para hacer uso de él.

![Captura de pantalla (507)](https://user-images.githubusercontent.com/69093629/121785296-4e6bad80-cbb9-11eb-9238-b74e9516e303.png)

Me encontré con el siguiente repositorio.

![Captura de pantalla (508)](https://user-images.githubusercontent.com/69093629/121785321-73f8b700-cbb9-11eb-94d3-71db240ad95e.png)

Me lo descargue.

```bash
┌─[root@parrot]─[/home/wackyhacker/Desktop]
└──╼ wget https://raw.githubusercontent.com/nikhil1232/CVE-2020-7384/main/CVE-2020-7384.sh

--2021-06-12 11:33:41--  https://raw.githubusercontent.com/nikhil1232/CVE-2020-7384/main/CVE-2020-7384.sh
Resolviendo raw.githubusercontent.com (raw.githubusercontent.com)... 185.199.109.133, 185.199.110.133, 185.199.111.133, ...
Conectando con raw.githubusercontent.com (raw.githubusercontent.com)[185.199.109.133]:443... conectado.
Petición HTTP enviada, esperando respuesta... 200 OK
Longitud: 2183 (2,1K) [text/plain]
Grabando a: “CVE-2020-7384.sh”

CVE-2020-7384.sh                                100%[====================================================================================================>]   2,13K  --.-KB/s    en 0s      

2021-06-12 11:33:41 (14,3 MB/s) - “CVE-2020-7384.sh” guardado [2183/2183]
```

Lo ejecute y me cree una template maliciosa por "netcat", hice uso del puerto 443 más la IP de la tun0 [10.10.16.5], de nombre le asigné exploit.apk.

![Captura de pantalla (509)](https://user-images.githubusercontent.com/69093629/121785375-cb972280-cbb9-11eb-9c09-18928d027278.png)

La subí en el servidor, puse mi IP en el buffer y seleccioné Android como sistema operativo.

![Captura de pantalla (510)](https://user-images.githubusercontent.com/69093629/121785521-7c052680-cbba-11eb-9432-d6ef1b19c50f.png)

Y me otorgo una shell por Netcat.

![Captura de pantalla (511)](https://user-images.githubusercontent.com/69093629/121785549-b7075a00-cbba-11eb-80f7-cc9eef28fa74.png)

La "flag" del usuario se encontraba en */home/kid/user.txt*, le hice un *cat* para visualizarla.

![voam](https://user-images.githubusercontent.com/69093629/121785702-9e4b7400-cbbb-11eb-994e-8d7dd9b58e3c.jpg)

Hice un tratamiento de la *TTY* para estar más cómodo.

![Captura de pantalla (512)](https://user-images.githubusercontent.com/69093629/121785592-f46be780-cbba-11eb-9a75-303212eb5eb1.png)

En */home/pwn* me encontré un script llamado scanlosers.sh, vi que es lo que hacía.

![Captura de pantalla (514)](https://user-images.githubusercontent.com/69093629/121785788-203b9d00-cbbc-11eb-8c35-9088ee75e64f.png)

Estaba declarando la variable log con una ruta absoluta del sistema */home/kid/logs/hackers*, después accede a */home/pwn*, hace un filtro de *log*, tras eso ejecuta una sesión de Nmap concatenando la variable *ip* y finalmente hace una comparación de "si es mayor que 0" las líneas que contiene la variable *log*.
Me dirigí a */home/kid/logs/hackers* y comencé a probar inyecciones de comandos basándonos en la programación del script, hasta que logre dar con uno que me ejecutaba el comando que quería, forzaba la ejecución del siguiente comando haciendo uso de ";" y el comando que yo quería, el output lo redirigí al archivo hackers que era donde apuntaba el script, también he comentado lo siguiente para que no haya ningún problema, ejecute el comando *whoami* como prueba y la respuesta fue pwn (**el archivo hackers no tenía capacidad de lectura**).

![Captura de pantalla (515)](https://user-images.githubusercontent.com/69093629/121786129-419d8880-cbbe-11eb-8da4-584cfb15c165.png)

Me entablé una reverse shell por Netcat.

![Captura de pantalla (516)](https://user-images.githubusercontent.com/69093629/121786245-e7e98e00-cbbe-11eb-977d-a96dc36a99a3.png)

Y me convertí en el usuario pwn, solo faltaba escalar privilegios, verifique si podía ejecutar algo como ROOT y por mi sorpresa tenía la capacidad de ejecutar el binario de Metasploit como el usuario ROOT, tan solo ejecuté ```sudo``` más el binario de Metasploit en */root/root.txt* estaba la *flag*.

![Captura de pantalla (517)](https://user-images.githubusercontent.com/69093629/121786348-9e4d7300-cbbf-11eb-9bd3-f036886b4e55.jpg)



