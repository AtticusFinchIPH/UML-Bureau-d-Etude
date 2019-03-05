# Protocole de Test

1. Objetive du document

Ce document fournit une description sur la structure de l'application, ensuite donner les instructions pour implémenter et démarrer le projet de Circulation Interlligente.



2. Objective de l'application

Circulation Interlligente est un système qui permet à circuler interligemment (par le bias des robot de type rover) dans un atelier afin de répondre à une mission donnée. Le système se base sur la détection de position de robots suite à l'utilisation des LIDARs et la collection des informations issues de la géo-localisation de type Indoor en utilisant le bornes WIFI de l’école.



3. Structure générale du programme

Pourque le robot se circule dans un bâtiment, il faut qu'il identifie sa localisation locale et en même temps, il essaie de collecter les obstacles autour de lui pour les éviter et suivre un bon chemin.

Pour ce but, on décide à déviser le project entre deux grandes parties : une partie de GPS Indoor (reconnaitre la localisation locale) et une autre de Lidar (constater les obtacles autour le robot).

La partie de GPS Indoor contient deux sous-parties:

- Client : (dossier be_v1) qui embarque sur le Raspberry. Son fonctionnement est de récupérer des signaux de réseau wifi en temps réel, puis les transmettre au serveur.
- Serveur : (dossier GPS_Indoor) qui importe sur l'ordinateur. Après avoir reçu les signaux du Raspberry, elle analyse, détermine la position actuelle du robot puis la montre sur l'écran via une interface d'utilisateur. 

![GPSIndoor Class Diagram](/home/tranv/Documents/BE/UML-BE/GPSIndoor/GPSIndoor Class Diagram.jpg)



4. Procédure générales pour démarrer l'application :

     4.1 Mesure les signaux autour du bâtiment.

     La première étape est de mesurer la puissance de réseau wifi autour du bâtiment. Cette étape nous aide à identifier d'abord le nombre de Point Access dans le bâtiment.

   De ce fait, il faut utiliser un capteur pour récupérer les signaux aux différents endroids dans le bâtiment. Chaque mesure dure environ 15 secondes.

     4.2 Enregistrement les Point Access

   En ce moment, on enregistre manuellement l'identité, la puissance et l'erreur de chaque Point Access sous les fichiers "FormuleXXX.java" dans le package Distance. Pourtant, dans le futur, on peut améliorer cette partie pour que les données de Point Access puissent enregistrer automatiquement et soient réutilisable.

   Ces informations des Point Access servent le serveur à calculer la position du robot après chaque mesure via la puissance de réseau où le robot recevoit.

   En même temps, on importe la carte du logement et indique les locations des Point Access sur cette carte.

   4.3 Récupération la puissance de Wifi

   Après avoir préparé les deux étapes au-dessus, on peut commencer à démarrer le système. En circulant dans l'atelier, le robot envoie les données à l'ordinateur chaque une période de temps. ces données contiennent les informations sur la puissance que le robot recevoit de chaque Point Access. A partir des **RSSI (Received Signal Strength Indication)** http://tdmts.net/2017/02/04/using-wifi-rssi-to-estimate-distance-to-an-access-point/, les distances entre le robot et les routeurs sont calculées.

   Sur la carte, le système nous montre des réseaux sous forme des bagues. Le zone où des bagues se croisent est la position estimée de notre robot.