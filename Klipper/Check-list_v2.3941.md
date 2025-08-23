# Check-list pour une bonne installation

## MA CONFIGURATION

* Raspberry PI5
* Carte mère BTT Octopus
* EBB42 v1.2
* Beacon3D
* Hotend Bambulab
* StealthMax

<hr>


## INSTALLATION AVEC RASPBERRY PI IMAGER
* __Modèle de Raspberry :__ selon votre modèle
* __Système d'exploitation :__ voir images ci-dessous

<center><img src="Images\Raspberry Pi Imager 1.png"></center><br>
<center><img src="Images\Raspberry Pi Imager 2.png"></center>
<hr>

## INSTALLATION KLIPPER

Après avoir insérer la carte SD dans votre rasperry, allumer votre imprimante et lancer l'application Putty pour piloter en SSH votre raspberry

### Mise à jour Kernel stable linux : 

    sudo apt-get update && sudo apt-get full-upgrade -y

<hr>

### 	Installation des différents outils : 

    sudo apt-get install git -y && sudo apt-get install dfu-util -y


<hr>

### 	Installation de [Kiauh](https://github.com/dw-0/kiauh)

Pour télécharger ce script, il est nécessaire d'avoir installé git. Si vous n'avez pas installé git, ou si vous n'êtes pas sûr, exécutez la commande suivante :

    sudo apt-get update && sudo apt-get install git -y

Une fois git installé, utilisez la commande suivante pour télécharger KIAUH dans votre répertoire personnel :

    cd ~ && git clone https://github.com/dw-0/kiauh.git

Démarrer KIAUH en copiant la commande ci-dessous :

    ./kiauh/kiauh.sh


* Installation de Klipper (1), Moonraker (2), Mainsail (3), KlipperScreen (5) et Crowsnest (14)
* _En option : Spoolman (13) pour la [Box Turtle](https://github.com/ArmoredTurtle/BoxTurtle/)_

<center><img src="Images\kiauh.png"></center>

Rebooter votre Raspberry et redémarrer votre session

<center><img src="Images\putty 1.png"></center>

## INSTALLATION CANBUS EBB42

### Sur votre imprimante

Avant de commencer, assurez-vous que votre imprimante est éteinte.

1. Insérer la prise USB-C sur la carte EBB42 (ou similaire) et sur votre raspberry pour assurer une connexion
<center><img src="Images\CanBus 1.png"></center>

2. Insérer le jumper sur la carte
<center><img src="Images\CanBus 2.png"></center>

3. Appuyer sur le bouton DFU __en même temps__ que vous allumer votre imprimante
<center><img src="Images\CanBus 3.png"></center>

### Installation du CANBUS

Les informations suivantes sont reprises du site [Esoterical](https://canbus.esoterical.online/). A suivre pas à pas les indications :

1. Préparation de l'installation du CANBUS

    sudo systemctl enable systemd-networkd
    sudo systemctl start systemd-networkd
    systemctl | grep systemd-networkd

XXX










































<hr>

## INSTALLATION DE LA SONDE [BEACON 3D](https://docs.beacon3d.com/)

### Installation du module Klipper du Beacon
Clonez `beacon_klipper` depuis git et exécutez le script d'installation :

    cd ~
    git clone https://github.com/beacon3d/beacon_klipper.git
    ./beacon_klipper/install.sh
	
### Configuration Moonraker Update Manager (en option)¶
Si votre imprimante fonctionne avec Moonraker, vous pouvez configurer le gestionnaire de mise à jour pour Beacon.
Vous devrez ajouter la section suivante au fichier `moonraker.conf` :

    [update_manager beacon]
    type: git_repo
    channel: dev
    path: ~/beacon_klipper
    origin: https://github.com/beacon3d/beacon_klipper.git
    env: ~/klippy-env/bin/python
    requirements: requirements.txt
    install_script: install.sh
    is_system_service: False
    managed_services: klipper
    info_tags:
      desc=Beacon Surface Scanner


### Calibration du Beacon3D

Faites un homing des axes X et Y :

    G28 X Y

Positionner votre buse au centre du plateau chauffant. Vous devrez ajuster les coordonnées de votre machine, ou n'hésitez pas à utiliser l'interface web.

    G0 X175 Y145

Démarrer le process de calibration :

    BEACON_CALIBRATE

Procéder au réglage de la distance entre votre buse et votre plateau chauffant.
La distance est équivalent d'une feuille de papier. Vous pouvez aussi utilisez des clinquants de 0.15 d'épaisseur :

    TESTZ Z=-0.01

Lorsque vous êtes satisfait de la resistance entre votre feuille de papier ou de votre clinquant, accepter pour terminer :

    ACCEPT

La réponse du capteur sera automatiquement mesurée et adaptée à un modèle. Enregistrez les résultats dans votre fichier de configuration :

    SAVE_CONFIG

<hr><hr>

### 	Installation de [Klippain](https://github.com/Frix-x/klippain)

Lancer la procédure d'installation

    wget -O - https://raw.githubusercontent.com/Frix-x/klippain/main/install.sh | bash

1. Confirmer le souhaite d'installer Klippain par yes `y`
2. Sélection et installation du MCU : `y`
3. Choisir votre carte mère : pour ma part `BTT_Octopus (21)`
4. Carte CanBus EBB42 v1.2 :  pour ma part `BTT EBB36-42 v1.2 (10)`
5. Carte MMU/ERCF :  pour ma part `BTT_MBB_CAN v1.0 (5)`

<hr>

### 	Installation de Shake&Tune
Installez Shake&Tune en l'exécutant via SSH sur votre imprimante :

    wget -O - https://raw.githubusercontent.com/Frix-x/klippain-shaketune/main/install.sh | bash

<hr>

## GESTION DES LEDS
* [Led Effect](https://github.com/julianschill/klipper-led_effect)

### Installation de Led Effect :

    cd ~
    git clone https://github.com/julianschill/klipper-led_effect.git
    cd klipper-led_effect
    ./install-led_effect.sh

<hr>

## KLIPPER TMC AUTOTUNE

### Installation de TMC Autotune :

    wget -O - https://raw.githubusercontent.com/andrewmcgr/klipper_tmc_autotune/main/install.sh | bash

### Installation sur Klipper
A insérer dans le fichier `printer.cgf`
	
    [autotune_tmc stepper_x]
	motor: omc-17hs19-2004s1
	
	[autotune_tmc stepper_y]
	motor: omc-17hs19-2004s1
	
	[autotune_tmc stepper_z]
	motor: omc-17hs19-2004s1
	
	[autotune_tmc stepper_z1]
	motor: omc-17hs19-2004s1
	
	[autotune_tmc stepper_z2]
	motor: omc-17hs19-2004s1
	
	[autotune_tmc stepper_z3]
	motor: omc-17hs19-2004s1
	
	[autotune_tmc extruder]
	motor: bondtech-42H025H-0704A-005


### Mes moteurs actuels:
#### Moteur A/B

    [motor_constants omc-17hs19-2004s1]
    resistance: 1.4
    inductance: 0.003
    holding_torque: 0.59
    max_current: 2
    steps_per_revolution: 200

#### Moteur Z/Z1/Z2/Z3

    [motor_constants omc-17hs19-2004s1]
    resistance: 1.4
    inductance: 0.003
    holding_torque: 0.59
    max_current: 2
    steps_per_revolution: 200

#### Moteur Extrudeur (Bondtech LGX)

    [motor_constants bondtech-42H025H-0704A-005]
	#Bondtech LGX https://www.bondtech.se/downloads/TDS/Bondtech-LGX-Motor-42H025H-0704-002.pdf
	resistance: 4.4
	inductance: 0.0055
	holding_torque: 0.16
	max_current: 0.7
	steps_per_revolution: 200

<hr>

### Fichiers customisés

Intégrer mes fichiers suivants :
* Répertoire `_MaConfig`
* fichier `override.cfg`

<hr>