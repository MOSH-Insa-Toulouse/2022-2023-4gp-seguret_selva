# 2022-2023-4gp-seguret_selva
## Notre projet MOSH - julien &amp; clément  
  
Dans le cadre de l'UF "Du capteur au banc de test en Open Source Hardware", nous avons été amenés à réaliser un capteur de déformation low-tech à base de graphite
Le fonctionnement de ce capteur est basé sur la variation de résistance engendrée par la déformation de celui-ci.
La physique derrière cette variation de résistance vient du déplacement des électrons par effet tunnel entre les nanoparticules de graphite.
Après avoir appliqué une contrainte mécanique, la distance entre les nanoparticules varie, entraînant donc une modification de la conductivité du graphite.
Cela induit donc une variation de résistance du capteur.  
  
Voici une image représentant notre capteur low-tech à base de graphite :  
  
![Figure 1: Capteur Graphite low-tech](./Datasheet/CapteurGraphite.png "Figure 1: Capteur Graphite low-tech")  
  
Dans ce fichier nous allons décrire tous les procédés qui ont permis d'aboutir à la création de ce capteur.  
  
Table des matières :  
  
* [1. Cahier des charges](#PremiereSection)  
* [2. Simulation électronique du circuit transimpédance sur LTSpice](#DeuxiemeSection) 
* [3. Le design de notre PCB sur KiCAD](#TroisiemeSection)
* [4. Notre code Arduino](#QuatriemeSection)  
* [5. Notre application Bluetooth](#CinquiemeSection)
* [6. Le banc de test et l'élaboration de la Datasheet](#SixiemeSection)  
    
### *1. Cahier des charges* <a id="PremiereSection"></a>
  
Nous sommes en binôme pour réaliser ce projet qui comprend plusieurs différents livrables :  
* Un Shield PCB au préalable designé sur KiCAD  
* Une application Android fonctionnant par Bluetooth permettant de visualiser l'évolution de la résistance du capteur en temps réel  
* Un code Arduino permettant de calculer en temps réel la variation de résistance du capteur ainsi que de contrôler les élements du PCB  
* Une datasheet regroupant les caractéristiques de fonctionnement du capteur  
  
Ainsi, afin de réaliser ce projet, il nous a été nécessaire de réaliser la simulation du capteur sur le logiciel LTSpice pour avoir une idée de l'ordre de grandeur de la résitance de celui-ci. 
Nous avons ensuite utlisé le logiciel KiCAD dans le but de designer le Shield PCB relatif au bon fonctionnement du capteur, et redigé un code Arduino
permettant le contrôle de certains éléments du Shield tels que le Flex Sensor, le module Bluetooth et l'écran OLED. Ce code Arduino nous permet également de calculer 
en temps réel la valeur de la résistance du capteur.
Après avoir effectué tout cela, nous avons créé une application APK Android permettant de connecter le module Bluetooth du PCB au téléphone de Julien, et de
visualiser l'évolution de la résistance du capteur sur un graphique en temps réel.
Enfin, la dernière étape visait à la réalisation d'une datasheet, via la caractérisation de notre jauge de contrainte.  
  
Le matériel suivant a été nécessaire à la confection de notre projet :  
* Une carte Arduino UNO    
* Un module Bluetooth HC-05  
* Un écran OLED I2C  
* Un amplificateur opérationnel LTC1050  
* Deux résistances de 100 KOhms  
* Une résistance de 10 KOhms  
* Deux résistances de 1 KOhm  
* Deux condensateurs de 100 nF  
* Un condensateur de 100 µF  
* Une feuille de papier  
* Cinq crayons à papier différents  
  
### *2. Simulation électronique du circuit transimpédance sur LTSpice* <a id="DeuxiemeSection"></a> 
  
Comme nous l'avons démontré dans le document nommé [Rapport LTSpice](https://github.com/MOSH-Insa-Toulouse/2022-2023-4gp-seguret_selva/blob/main/LTSpice/Rapport%20LTSpice.pdf), le capteur ne délivrait qu'une intensité très faible (de l'ordre de quelques nF). Aussi, la carte Arduino n'est capable de sortir qu'une tension entre 0 et 5V.  
Ainsi, il nous est apparu comme nécessaire de réaliser un circuit transimpédance afin de convertir les variations de courant du capteur en variations de tension, de sorte
que la carte Arduino puisse la lire en temps réel. En parallèle de tout ça, sur ce circuit, nous avons placé quelques filtres passe-bas nous permettant de réduire les bruits
liés à la mesure, et nous permettant à l'aide d'un amplificateur LTC1050 d'amplifier le signal de sortie.  
  
Voici, la simulation de notre amplificateur transimpédance réalisé sur le logiciel LTSpice :  
  
![Figure 2: Amplificateur Transimpédance](./LTSpice/Ampli_Transimpedance.png "Figure 2: Amplificateur Transimpédance")  
  
### *3. L'utilisation de KiCAD en vue de la création de notre PCB* <a id="TroisiemeSection"></a>  
  
Dans l'objectif de réaliser notre PCB, nous avons dû utiliser le logiciel KiCAD. Certains composants n'étant pas disponibles dans les librairies du logiciel, 
nous les avons créés afin de pouvoir designer tous les éléments dont nous avions besoin pour réaliser le Shield. Ce sont les éléments suivants accompagnés
de leur empreinte et de leur schéma :  
  
* Le module Bluetooth HC-05 :  
![Figure 4: Module Bluetooth Schéma](./KiCAD/module_bt_schem.png "Figure 4: Module Bluetooth Schéma") ![Figure 5: Module Bluetooth Empreinte](./KiCAD/module_bt_foot.png "Figure 5: Module Bluetooth Empreinte") 
* L'amplificateur opérationnel LTC1050 :   
![Figure 6: LTC1050 Schéma](./KiCAD/LTC_schem.png "Figure 6: LTC1050 Schéma") ![Figure 7: ALTC1050 Empreinte](./KiCAD/LTC_foot.png "Figure 7: LTC1050 Empreinte") 
* L'écran OLED I2C :  
![Figure 8: OLED I2C Schéma](./KiCAD/oled_schem.png "Figure 8: OLED I2C Schéma") ![Figure 9: OLED I2C Empreinte](./KiCAD/oled_foot.png "Figure 9: OLED I2C Empreinte") 
* Le Flex Sensor :  
![Figure 10: Flex Sensor Schéma](./KiCAD/flex_schem.png "Figure 10: Flex Sensor Schéma") ![Figure 11: Flex Sensor Empreinte](./KiCAD/flex_foot.png "Figure 11: Flex Sensor Empreinte")  
* Le capteur graphite :  
![Figure 14: Capteur Graphite Schéma](./KiCAD/graphite_sens_schem.png "Figure 14: Capteur Graphite Schéma") ![Figure 15: Capteur Graphite Empreinte](./KiCAD/graphite_sens_foot.png "Figure 15: Capteur Graphite Empreinte")
  
Vous pouvez également visualiser ici le schéma de notre Shield Arduino ainsi que de la PCB :  
  
![Figure 12: Schéma Complet](./KiCAD/Schem_complet.png "Figure 12: Schéma Complet") ![Figure 13: PCB](./KiCAD/pcb_final.png "Figure 13: PCB")  
   
### *4. Notre code Arduino* <a id="QuatriemeSection"></a>  
  
Notre code Arduino nous a permis de connecter tous les éléments du Shield entre eux afin d'assurer le bon fonctionnement de celui-ci.  
Rappelons que notre code Arduino que l'on peut retrouver dans le document nommé [AffichageRes](https://github.com/MOSH-Insa-Toulouse/2022-2023-4gp-seguret_selva/blob/main/Code%20Arduino/AffichageRes) prend en charge les fonctionnalités suivantes :  
* L'envoi et la réception de données sur l'application APK via le module Bluetooth HC-05  
* Le calcul en temps réel de la variation de résistance associée à la déformation du capteur graphite  
* L'acquistion et l'affichage de cette valeur de résistance sur l'écran OLED  
* Le lancement de l'acquisition de données pour une certaine valeur de déformation du Flex Sensor  
  
  
### *5. Notre application Bluetooth* <a id="CinquiemeSection"></a> 
  
Notre application Andoid a été développée à l'aide du logiciel *MIT App Inventor*. Nous avons créé une interface graphique téléchargeable sur Android afin de :  
* Connecter une téléphone au PCB via le module Bluetooth  
* Visualiser en temps réel sous forme de graphique l'évolution de la résistance en fonction de la déformation  
* Afficher la résistance du capteur en temps réel  
  
### *6. Le banc de test et l'élaboration de la Datasheet* <a id="SixiemeSection"></a>  
  
Comme nous avons créé un capteur graphite low-tech, nous avons également décidé d'utiliser un banc de test low-tech.
Le banc de test que vous pouvez voir ci-dessous a été créé il y a quelques années par l'un des professeurs de l'INSA Toulouse.  
  
![Figure 3: Banc de Test](./Datasheet/Banc_De_Test.png "Figure 3: Banc de Test")  
  
Ce banc de test nous a permis de tester différents angles de courbure pour notre capteur. En effet, nous avons placé notre capteur graphite sur les différentes
portions de cette pièce, afin de faire varier sa résistance, que ce soit en tension ou en compression.
  
L’objectif de ce banc de test est de déterminer la variation relative de résistance de notre capteur en fonction de la déformation appliquée. Le capteur graphite est quant à lui scotché par une extrémité sur une autre feuille rigide, et pincé au niveau des pins de l’autre extrémité. L’objectif est de courber la feuille de papier rigide sur laquelle est scotché le capteur, afin que celui-ci adopte la forme des cercles de différents rayons.

L'ensemble de ces tests ont été réalisés pour 5 types de crayons différents. Les résultats et analyses de ceux-ci sont disponibles sur le document suivant nommé :
[Datasheet jauge](https://github.com/MOSH-Insa-Toulouse/2022-2023-4gp-seguret_selva/blob/main/Datasheet/Datasheet%20jauge.pdf)  
  
Il faut noter que notre capteur fonctionne assez bien car, pour n'importe quel type de crayon, la variation relative de résistance de notre capteur en fonction de la 
déformation appliquée est quasi-linéaire.  
  
Malheureusement, il est notable que notre capteur graphite s'endommage assez rapidement lorsque l'on le plie car le papier n'est pas solide. Il n'est donc pas
possible de réaliser la mesure plusieurs fois afin de vérifier ses résultats, ce qui rend la datasheet assez imprécise. Cela se traduit également par le fait que 
les courbes obtenues ne sont pas totalement linéaires. Aussi, le dépôt de graphite lors du coloriage du papier est assez aléatoire, rendant le capteur
encore plus imprécis.  
  
Tous ces éléments nous laissent ainsi penser que ce capteur low-tech est réalisable, mais n'est pas nécessairement très adapté à un usage industriel.
  

