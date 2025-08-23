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

Les informations suivantes sont reprises du site [Esoterical](https://canbus.esoterical.online/). A suivre pas à pas les indications :

### Préparation de l'installation du CANBUS

Assurez-vous que le service systemd-networkd est activé :

    sudo systemctl enable systemd-networkd

Démarrez le service :

    sudo systemctl start systemd-networkd

Vérifiez qu'il fonctionne correctement :

    systemctl | grep systemd-networkd

Assurez-vous qu'il s'affiche comme « loaded active running ».

<center><img src="Images\loaded active running.png"></center>

Désactiver complètement le service wait-online :

    sudo systemctl disable systemd-networkd-wait-online.service
	
Configurez « txqueuelen » pour l'interface :

    echo -e 'SUBSYSTEM=="net", ACTION=="change|add", KERNEL=="can*"  ATTR{tx_queue_len}="128"' | sudo tee /etc/udev/rules.d/10-can.rules > /dev/null

Vérifier si l'application s'est correctement exécutée

    cat /etc/udev/rules.d/10-can.rules
 
Cela devrait ressembler à ceci :
<center><img src="Images\10-can rules.png"></center>

Enfin, pour activer l'interface can0 et définir la vitesse, exécutez la commande suivante :

    echo -e "[Match]\nName=can*\n\n[CAN]\nBitRate=1M\nRestartSec=0.1s\n\n[Link]\nRequiredForOnline=no" | sudo tee /etc/systemd/network/25-can.network > /dev/null

Vérifier si l'application s'est correctement exécutée

    cat /etc/systemd/network/25-can.network

Cela devrait ressembler à ceci :
<center><img src="Images\25-can network.png"></center>

Rebootez votre Raspberry

    sudo reboot now

### Flashage de la carte mère en mode USB CAN Bridge avec KATAPULT

Eteindre votre imprimante et insérer un jumper (en violet) sur la carte mère pour le passer en mode DFU
<center><img src="Images\DFU_octopus.png"></center>

Il faut installer certaines dépendances :

    sudo apt update
    sudo apt upgrade
    sudo apt install python3 python3-serial

Installez KATAPULT

    test -e ~/katapult && (cd ~/katapult && git pull) || (cd ~ && git clone https://github.com/Arksine/katapult) ; cd ~

Configurez KATAPULT selon cette image
<center><img src="Images\Katapult Config.png"></center>

    cd ~/katapult
    make menuconfig

Confilez le firmware

    make clean
    make

Vérifier si la carte est bien en mode DFU

    lsusb
	
<center><img src="Images\confirmation DFU_octopus.png"></center>

Si ce n'est pas le cas, appuyer sur le bouton RESET (en vert sur la vue représentant la carte mère)

Si c'est le cas

    cd ~/katapult
    make
    sudo dfu-util -R -a 0 -s 0x08000000:mass-erase:force:leave -D ~/katapult/out/katapult.bin -d 0483:df11

Retirez le jumper et faire un reset. Vérifier si la carte est identifiée en KATAPULT

    ls /dev/serial/by-id

Je devrais voir `/dev/serial/by-id/usb-katapult_stm32f446xx_260056001251373234333632-if00`
<center><img src="Images\octopus_identifie_katapult.png"></center>

Si ce n'est pas identifié, [recommencer la procédure](https://github.com/Itzo1978/Voron-2.4/blob/main/Klipper/Check-list_v2.3941.md#flashage-de-la-carte-m%C3%A8re-en-mode-usb-can-bridge-avec-katapult)

### Flashage de la carte mère en mode USB CAN Bridge avec KLIPPER

Configurez KLIPPER selon cette image
<center><img src="Images\Klipper USB-CAN-Bridge Config.png"></center>

    cd ~/klipper
	make menuconfig

Confilez le firmware

    make clean
    make

Utiliser KATAPULT pour flasher KLIPPER

    sudo service klipper stop

Lancer la commande pour flasher la carte mère 

    python3 ~/katapult/scripts/flashtool.py -f ~/klipper/out/klipper.bin -d /dev/serial/by-id/usb-katapult_stm32f446xx_260056001251373234333632-if00

Vérifier si Geschwister Schneider CAN adapter apparait

    lsusb

<center><img src="Images\Geschwister Schneider CAN adapter.png"></center>

Vérifiez que l'interface can0 est active :

    ip -s -d link show can0

<center><img src="Images\can0 active.png"></center>

Lancer la requête pour récupérer le canbus_uuid de votre carte mère :

    ~/klippy-env/bin/python ~/klipper/scripts/canbus_query.py can0
	
Je devrais voir `Detected UUID: 147d4dc27d4a, Application: Klipper`

Relancer KLIPPER

    sudo service klipper start


### Flashage de la Toolhead avec KATAPULT

Eteindre votre imprimante
Brancher le cable USB du Raspberry au Toolhead
S'assurer le cable umbilical toolhead soit déconnecté

Appuyer sur le bouton RESET de la Toolhead en rallumant l'imprimante

Il faut installer certaines dépendances :

    sudo apt update
    sudo apt upgrade
    sudo apt install python3 python3-serial

Installez KATAPULT

    test -e ~/katapult && (cd ~/katapult && git pull) || (cd ~ && git clone https://github.com/Arksine/katapult) ; cd ~

Configurez KATAPULT selon cette image
<center><img src="Images\Katapult Config Toolhead.png"></center>

    cd ~/katapult
    make menuconfig

Confilez le firmware

    make clean
    make

Vérifier si la Toolhead est bien en mode DFU

    lsusb
	
<center><img src="Images\confirmation DFU_toolhead.png"></center>

Si ce n'est pas le cas, éteindre l'imprimante et appuyer sur le bouton RESET de la Toolhead en rallumant l'imprimante

Si c'est le cas

    sudo dfu-util -R -a 0 -s 0x08000000:mass-erase:force:leave -D ~/katapult/out/katapult.bin -d 0483:df11

Eteindre l'imprimante, retirer totalement le cable USB, connecter le cable umbilical sur la toolhead et allumer l'imprimante

Exécutez la commande suivante pour vérifier si la carte de la tête d'outil est connectée au réseau CAN et en mode Katapult.

    python3 ~/katapult/scripts/flashtool.py -i can0 -q

Je devrais voir 2 lignes identiques :
* `Detected UUID: 147d4dc27d4a, Application: Klipper` 
* `Detected UUID: 1469b906a561, Application: Katapult`

`147d4dc27d4a` correspond à la carte mère
`1469b906a561` correspond à la Toolhead

Il est important que le 2ème UUID (`1469b906a561`) doit être en KATAPULT et non en KLIPPER. Si ce n'est pas le cas, [recommencer la procédure](https://github.com/Itzo1978/Voron-2.4/blob/main/Klipper/Check-list_v2.3941.md#flashage-de-la-toolhead-avec-katapult).

### Flashage de la Toolhead avec KLIPPER

Configurez KLIPPER selon cette image
<center><img src="Images\Klipper Toolhead Config.png"></center>

    cd ~/klipper
	make menuconfig

Confilez le firmware

    make clean
    make

Utiliser KATAPULT pour flasher KLIPPER

    sudo service klipper stop

Lancer la commande pour flasher la carte mère 

    python3 ~/katapult/scripts/flashtool.py -i can0 -f ~/klipper/out/klipper.bin -u 1469b906a561

Vérifier si tous sont en mode KLIPPER

    python3 ~/katapult/scripts/flashtool.py -i can0 -q

<center><img src="Images\UUID.png"></center>

Ces deux UUID sont à utiliser dans la section [mcu] du fichier de configuration de l'imprimante

Redémarrez le service Klipper

    sudo service klipper start

<hr>

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

<hr>

### Fichiers customisés

Intégrer mes fichiers suivants :
* Répertoire `_MaConfig`
* fichier `override.cfg`

<hr>