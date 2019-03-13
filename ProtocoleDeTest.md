# Protocole de Test

1. Objetif du document

Ce document fournit une description sur la structure de l'application, et donner les instructions pour implémenter et démarrer le projet de Circulation Interlligente.



2. Objectif de l'application

Circulation Interlligente est un système qui permet à circuler interligemment (par le bias des robot de type rover) dans un atelier afin de répondre à une mission donnée. Le système se base sur la détection de la position du robot grace à l'utilisation conjointe du LIDAR et la collection des informations issues de la géo-localisation de type Indoor en utilisant le bornes WIFI de l’école.



3. Structure générale du programme

Pourque le robot se circule dans un bâtiment, il lui sa position locale ainsi que les obstacles autour de lui pour les éviter et suivre un bon chemin.

Dans ce but, on décide à déviser le project entre deux grandes parties : une partie de GPS Indoor (reconnaitre la localisation locale) et une autre de Lidar (constater les obtacles autour le robot).

La partie de GPS Indoor contient deux sous-parties:

- Client : (dossier client_raspberry_indoorGPS) qui est embarqué sur le Raspberry. Son role est de récupérer des signaux de réseau wifi en temps réel, puis les transmettre au serveur.
- Serveur : (dossier serveur_interface_indoorGPS) qui importe sur l'ordinateur. Après avoir reçu les signaux du Raspberry, il analyse, détermine la position actuelle du robot puis l'affiche sur l'écran via une interface d'utilisateur. 

![GPSIndoor Class Diagram](/home/tranv/Documents/BE/UML-BE/GPSIndoor/GPSIndoor Class Diagram.jpg)



4. Procédure générales pour démarrer l'application :

     4.1 Mesure les signaux autour du bâtiment.

     La première étape est de mesurer la puissance de réseau wifi autour du bâtiment. Cette étape nous aide à identifier d'abord le nombre de Point Access dans le bâtiment.

   De ce fait, il faut utiliser un capteur (antene wifi) pour récupérer les signaux aux différents endroids dans le bâtiment. Chaque mesure dure environ 15 secondes.

   4.2 Enregistrement les Points Access

   Pour le moment, on enregistre manuellement l'identité, la formule de conversion Puissance/Distance de chaque point d'accès dans les classes "FormuleXXX.java" qui correspondent a un patern Strategy.

   Ces informations des Point Access servent le serveur à calculer la position du robot après chaque mesure via la puissance de réseau où le robot recevoit.

   En même temps, on importe la carte du logement et indique les locations des Point Access sur cette carte.

   4.3 Récupération la puissance de Wifi

   Après avoir préparé les deux étapes au-dessus, on peut commencer à démarrer le système. En circulant dans l'atelier, le robot envoie les données à l'ordinateur chaque une période de temps. Ces données contiennent les informations sur la puissance que le robot recevoit de chaque Point Access. A partir des **RSSI (Received Signal Strength Indication)** http://tdmts.net/2017/02/04/using-wifi-rssi-to-estimate-distance-to-an-access-point/, les distances entre le robot et les routeurs sont calculées.

   Sur la carte, le système nous montre des réseaux sous forme des bagues. Le zone où les anneaux se croisent est la position estimée de notre robot.