---
title:  "Hacking Redes WPA/WPA2-PSK"
header:
  teaser: "https://miro.medium.com/max/1875/1*eFAO_PfEpLHCDtMTyrrrbg.png"
categories: 
  - Wi-Fi
tags:
  - Fluxion
  - Aircrack-ng
---

<p align="center">
<img src="https://miro.medium.com/max/1875/1*eFAO_PfEpLHCDtMTyrrrbg.png">
</p>

En este articulo, nos centraremos en aprender como se pueden vulnerar Redes WI-FI con cifrado WPA/WPA2-PSK, y conseguir la contraseña de la misma. Para ello, os enseñare unos cuantos ataques muy interesantes que podéis efectuar siempre y cuando sea en un entrono controlado.

Debéis contar con 3 condiciones anteriormente, tener un ordenador con un sistema Linux instalado (da igual cual), una antena que acepte el modo monitor, y las ganas de aprender, la ultima es muy importante!!!.

“**WI-FI**”, en palabras resumidas, es una tecnología que permite la conexión de distintos dispositivos a ella, para brindarles servicios de Internet (Puede no tener Internet). En las Redes WI-FI existen distintos tipos de cifrados que se encargan de encriptar la contraseña y así que no sea “crackeada” tan fácilmente por terceros, hasta el día de hoy las Redes WI-FI cuentan con 3 tipos de cifrados diferentes, uno mas seguro que otro, "**WEP**”(**menos seguro**) “**WPA**” “**WPA2**” el primero esta casi obsoleto porque es antiguo y su seguridad es pésima, y los dos últimos son los que mas se utilizan a día de hoy.

Bueno ahora que ya sabemos ¿Qué es una Red WI-FI?, y ¿Qué tipos de cifrados las componen?, vayamos a lo que nos interesa, ¿Cómo se puede Crackear una Red WI-FI?.
Para ello voy a empezar hablando de dos técnicas muy utilizadas en en el cracking de Redes WI-FI (la primera suele ser mas efectiva).

El Phishing y la Fuerza bruta de contraseñas, la primera consiste en engañar a la persona para que introduzca la contraseña en una pagina que se hace pasar por la original (haciendo ingeniería social), la segunda se basa en ir probando contraseñas a una velocidad sobrehumana hasta dar con la correcta.
Antes de empezar la ejecución de estas dos técnicas, un bonus para falsificar vuestra MAC y sea mas difícil detectar quien esta por detrás, ejecutáis lo siguiente haciendo uso de la herramienta “**macchanger**”:

```bash
root@parrot:# apt install macchanger -y #Desargar la herramienta en caso de no tenerla

root@parrot:# macchanger -r <InferfazDeRed> #Ex: macchanger -r wlan0
```

El segundo comando anterior lo que hace es cambiar la MAC de vuestra interfaz de red a una aleatoria.

## Primera técnica

Para la primera técnica vamos a hacer uso de “**Fluxion**”, una herramienta ofensiva de hacking de Redes WI-FI. Esta herramienta te brinda diferentes tipos de plantillas a utilizar, y por si fuera poco te crea un certificado SSL autofirmado para que pases mas desapercibido.

<p align="center">
<img src="https://miro.medium.com/max/875/1*pvUr-C12Q7R7_olDMH1ufA.jpeg">
</p>

Su uso es muy sencillo, lo primero que debemos hacer es clonar el repositorio de GitHub [https://github.com/FluxionNetwork/fluxion] para descargar la herramienta en nuestro sistema Linux y ya podríamos empezar con la post-explotación.

```bash
root@parrot:# git clone https://github.com/FluxionNetwork/fluxion #Clonar el repositorio
```

Una vez clonada, entramos en la carpeta de la herramienta y ejecutamos el script hecho en bash llamado fluxion.sh.

```bash
root@parrot:# cd fluxion #Para acceder a la carpeta

root@parrot:# chmod +x fluxion.sh #Dar permisos de ejecución al script

root@parrot:# ./fluxion.sh -i #Ejecucion del script && instalación de dependencias
``` 

Así se vería la herramienta ejecutada:

<p align="center">
<img src="https://miro.medium.com/max/875/1*PRrbdDTGc-WiV47hQL4T0w.png">
</p>

Como vemos tenemos dos opciones, la primera es para crear el Portal Cautivo, y la segunda es la captura del “**handshake**”, el apretón de manos o mas conocido como “**handshake**” no es mas que el intercambio de información privada y se captura en la “**deauthentication**” entre el cliente y el AP.

<p align="center">
<img src="https://miro.medium.com/max/610/1*lryoPcxhFhlZLWl6bN1sfw.png">
</p>

Todo esto nos lo automatiza “**Fluxion**”, pero se puede hacer perfectamente manualmente con “**aireplay-ng**”, hablaremos de esta maravillosa herramienta mas adelante.

Una vez elegida la opción 2 (**Handshake Snooper**), la herramienta nos empezara a iniciar algo llamado modo monitor, este modo nos permite capturar paquetes que viajan por la red y poder hacer lo queramos con ellos, sin una antena que acepte este modo no podremos llegar muy lejos, al final del articulo dejare unos links para que podáis adquirir una antena que acepte este modo.

Ahora nos va a pedir que elijamos el tipo de frecuencia de redes WI-FI a escanear, elegimos la opción correspondiente a nuestra antena, si no tenemos claro a que frecuencia puede conectarse nuestra antena elegimos la opción 3 y ya esta no hay problema.

<p align="center">
<img src="https://miro.medium.com/max/650/1*ewAwp7cOzVeMBVb3aZ0Vkg.png">
</p>

Y empezara con el escaneo de redes WPA/WPA2-PSK, esperamos de 30s a 60s a que nos escanee redes y pulsamos CTRL + C.

<p align="center">
<img src="https://miro.medium.com/max/875/1*ohG_P5Jnd1rQTdGeTyWQXw.png">
</p>

Ahora nos pedirá que elijamos una red a atacar, elegimos una con el numero correspondiente y pulsamos ENTER.

Nos pedirá que elijamos una interfaz de red, aquí utilizaremos la opción numero 1.

<p align="center">
<img src="https://miro.medium.com/max/640/1*rYsfoEiJ5VqXzyGenw4T9A.png">
</p>

Seguido nos pedirá el tipo de ataque a utilizar para la captura del handshake, recomiendo utilizar la segunda opción (**aireplay-ng deauthtentication**).

<p align="center">
<img src="https://miro.medium.com/max/620/1*4OMcIEG8EtLvgHvoGLCvMg.png">
</p>

Para el método de verificación recomiendo utilizar "**Pyrit**".

<p align="center">
<img src="https://miro.medium.com/max/601/1*CftwVsK-AoPoghEaUy6W1g.png">
</p>

Nos pedirá el tiempo a esperar por cada envió de paquetes para la “**deauthentication**”, elegimos la que nos convenga, yo utilizo 30s.

Y finalmente nos va a preguntar en caso de que ocurra… elegimos la segunda opción (**recomendable**).

Se nos abrirán 3 ventanas a la vez, cada una hace una función diferente, la de abajo a la derecha es la que se encarga de enviar paquetes repetidamente hasta capturar el “**handshake**”, la de abajo a la izquierda su función es capturar el “**handshake**”, y la ultima, que esta arriba, da información de cuantos clientes están autenticados en la red, a parte de su MAC y mas información.

<p align="center">
<img src="https://miro.medium.com/max/488/1*is6YbO32E8PbmQNVidRD3w.png">   
</p>                                                                      

<p align="center">
<img src="https://miro.medium.com/max/479/1*xTsw6bg7-80NHadprgh6vQ.png">
</p>

<p align="center">
<img src="https://miro.medium.com/max/483/1*qqKcBYHMSRMz1_iT9XbvtQ.png">
</p>

Una vez capturado el “**handshake**” seria cuestión de pulsar CTRL + C, y seleccionar la opción 1 (**Portal Cautivo**).

<p align="center">
<img src="https://miro.medium.com/max/1206/1*PRrbdDTGc-WiV47hQL4T0w.png">
</p>

Si nos dice “Fluxion is targetting the access point above” solo escribimos “Y” y problema arreglado, seguido nos dirá que seleccionemos nuestra interfaz de red, es muy importante seleccionar la opción que pone “**Skip**” ya que la seleccionaremos mas adelante.

Es probable que nos diga “This attack has already been configured”, aquí solo seria cuestión de seleccionar la opción 2 (Reset attack).

Ahora de nuevo nos pedirá que seleccionemos una interfaz de red, para ello seleccionamos la nuestra, en mi caso “wlan0".

<p align="center">
<img src="https://miro.medium.com/max/640/1*rYsfoEiJ5VqXzyGenw4T9A.png">
</p>

Nos dirá que seleccionemos el modo de “deauthentication”, recomiendo utilizar la segunda opción (aireplay), seguidamente nos pedirá que seleccionemos el servicio del AP a utilizar, yo hago uso de la primera opción (Rogue AP - hostapd), seguidamente nos dirá que ha encontrado el hash del AP y nos preguntara si queremos utilizar este archivo, en mi caso lo voy a utilizar seleccionando la opción que pone (Une hash found), de nuevo como en la captura del apretón de manos, nos preguntara por el tipo de verificación que queremos utilizar, en mi caso voy a utilizar “Pyrit”, ahora viene una de las mejores opciones que tiene la herramienta, el creado de un certificado SSL autofirmado, nos preguntara si queremos crear un certificado SSL, en mi caso es un rotundo “SI”, para ello hay que seleccionar la opción numero uno.

Ya solo queda seleccionar dos opciones, la primera opción pregunta si quieres desautenticar a los clientes de la red, y la segunda es la selección de la plantilla para la ingeniería social, la primera seleccione la opción numero 1, y en la segunda hice uso de la plantilla numero 23.

Y así se vería el ataque en acción:

<p align="center">
<img src="https://miro.medium.com/max/875/1*2B_HH2lrKeQkxqhaLUFlTA.png">
</p>

## Segunda técnica

En esta segunda técnica vamos a hacer uso de la suite de “Aircrack-ng”.

<p align="center">
<img src="https://miro.medium.com/max/686/1*8Yv2LbWO21sq9GOa7qJS_w.png">
</p>

“Aircrack-ng” es una suite de herramientas de hacking WI-FI, entre otras cosas permite al atacante crackear redes WI-FI por medio de un ataque de fuerza bruta de contraseñas a una velocidad sobrehumana. Dentro de lo que cabe su uso es sencillo (depende de tu conocimiento).

En esta ocasión utilizaremos 3 herramientas de la suite de “Aircrack-ng” mas la propia suite, la primera es “airmon-ng” que nos va a permitir poner nuestra interfaz de red en modo monitor (si lo acepta), “airodump-ng”, su función es el escaneo de redes inalámbricas, y “aireplay-ng” que se va a encargar de enviar paquetes para la “deauthentication”. ¿Estais preparados?, pues empezemos!!!.

Lo primordial para el hacking de redes WI-FI es poner la interfaz de red en modo monitor, para ello primero debemos matar a los procesos que nos lo impiden, suelen ser el dos, el primero “wpa_supplicant” y el segundo “dhcclient” para matarlos hacemos uso del siguiente comando:

```bash
root@parrot:# airmon-ng check kill
```

o si los queremos matar uno por uno:

```bash
root@parrot:# airmon-ng kill <PID> o <NombreDeProceso>
```

Una vez matados los procesos perderemos la conexión a Internet, para no perder la conexión hay que conectar el cable a la entrada RJ-45, ahora solo faltaría poner nuestra interfaz en modo monitor, para ello hacemos uso del comando:

```bash
root@parrot:# airmon-ng start <InterfazDeRed> #Ex: airmon-ng wlan0
```

Para verificar que nuestra de interfaz de red esta en modo monitor ejecutamos:

```bash
root@parrot:# iwconifig
```

Si nos sale el nombre de nuestra interfaz de red junto a “mon”, quiere decir que vamos por buen camino.

**Nota:** *Para este ataque haremos uso de un diccionario descargado previamente de GitHub.

Vamos a empezar escaneando la red haciendo uso de “airodump-ng” con la siguiente sintaxis:

```bash
root@parrot:# airodump-ng <InterfazDeRed+mon> #Ex: airodump-ng wlan0mon
```

<p align="center">
<img src="https://miro.medium.com/max/875/1*ohG_P5Jnd1rQTdGeTyWQXw.png">
</p>

Una vez ya tengáis bastantes redes a vuestra disposición una gran variedad de redes escaneadas, ejecutáis este comando a la red a atacar:

```bash
root@parrot:# airodump-ng -c <canal> --bssid <BSSID> -w psk <ESSID> #Ex: airodump-ng -c --bssid 90:CD:B6:83:43:B2 -w psk Oppo
``` 
  
Ahora necesitamos hacer la “deauthentication”, para ello vamos a hacer uso de la herramienta “aireplay-ng” esta herramienta nos va a permitir enviar paquetes a un cliente en especifico, o a toda la red si así lo preferimos para hacer la captura del apretón de manos, su sintaxis es la siguiente:

```bash
root@parrot:# aireplay-ng <0-”PaquetesAenviar”> -a <BSSID> -c <BSSIDClienteEspecifico> <InterfazDeRed> #aireplay-ng 0-10 -a 90:CD:B6:83:43:B2 -c 43:53:D4:34:D5:54 wlan0mon
```

**Nota:** *Puede no ser cuestión de tiempo la captura del “handshake”
  
Ahora si vamos a utilizar “Aircrack-ng”, la navaja suiza del hacking de Redes WI-FI, para esta ocasión vamos a utilizar un diccionario muy conocido llamado rockyou.txt, este diccionario contiene contraseñas de base de datos de todo tipo, por lo que lo hace una muy buena opción para los ataques.

Con el siguiente comando empezaríamos el ataque:

```bash
root@parrot:# aircrack-ng -w /usr/share/wordlists/rockyou.txt -b <BSSID> psk*.cap
``` 

**Nota:** *Este método no es tan efectivo como el primero, ya que este hace uso de un diccionario. Si la contraseña es robusta es posible que no se llegue a crackear, en cambio el otro da igual la complejidad que tenga la contraseña, es más por el sentido común de la víctima.
