# Check-list pour une bonne installation

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


* Installation de Klipper (1), Moonraker (2), Mainsail (3) et Crowsnest (14)
* _En option : Spoolman (13) pour l'[ERCF](https://github.com/Enraged-Rabbit-Community/ERCF_v2)_

<center><img src="Images\kiauh.png"></center>

Rebooter votre Raspberry et redémarrer votre session

<center><img src="Images\putty 1.png"></center>

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

## INSTALLATION CANBUS EBB42

### 	Sur votre imprimante

Avant de commencer, assurez-vous que votre imprimante est éteinte.

1. Insérer la prise USB-C sur la carte EBB42 (ou similaire) et sur votre raspberry pour assurer une connexion
<center><img src="Images\CanBus 1.png"></center>

2. Insérer le jumper sur la carte
<center><img src="Images\CanBus 2.png"></center>

3. Appuyer sur le bouton DFU __en même temps__ que vous allumer votre imprimante
<center><img src="Images\CanBus 3.png"></center>

### 	Dans SSH (avec Putty ou autre)

Connectez-vous à votre Imprimante en SSH.
Installez DFU_UTIL avec la commande suivante :

    sudo apt install dfu-util -y

Assurez vous que votre carte est bien détectée en mode DFU et taper `lsusb` pour identifier notre carte
<center><img src="Images\CanBus 4.png"></center>

Répeter cette opération jusqu'à l'identification de la carte qui se nomme `0483:df11` dans mon cas.

### 	CanBoot

Installation de CanBoot avec compilation pour notre carte MCU

    git clone https://github.com/Arksine/CanBoot
	cd CanBoot
	make menuconfig

Voici vue sur la configuration à effectuer
<center><img src="Images\CanBus 5.png"></center>

Quand c'est en ordre, quitter en tapant sur `Q` et confirmer par `Y`

Créer la compilation en tapant `make`.
<center><img src="Images\CanBus 6.png"></center>

On peut maintenant flasher le Bootloader qu'on a compilé

    sudo dfu-util -a 0 -d 0483:df11 --dfuse-address 0x08000000:force:mass-erase -D ~/CanBoot/out/canboot.bin

<center><img src="Images\CanBus 7.png"></center>

### 	Configuration du CAN sur le Raspberry
Installer nano si ce n'est pas déja fait

    sudo apt update && sudo apt install nano -y

Créer le fichier de configuration pour le port CAN. Copier Coller d'un bloc.

    sudo /bin/sh -c "cat > /etc/network/interfaces.d/can0" << EOF
    allow-hotplug can0
    iface can0 can static
     bitrate 250000
     up ifconfig \$IFACE txqueuelen 1024
    EOF

Ouvrez le fichier et vérifiez le code inséré :

    sudo nano /etc/network/interfaces.d/can0
	
Pour quitter, appuyer sur CTRL + X

Vous pouvez éteindre l'imprimante, retirer le cable USB et insérer le cable dédié

### 	Cablage du CAN Bus

Brancher tous les éléments sur la carte (cartouche, sonde, ventilateur) et allumer l'imprimante
<center><img src="Images\CanBus 8.png"></center>

### 	Flash Klipper sur EBB

Paramétrer Klipper selon la carte EBB utilisée

    cd ~/klipper
    make menuconfig

Voici vue sur la configuration à effectuer
<center><img src="Images\CanBus 9.png"></center>

Quand c'est en ordre, quitter en tapant sur `Q` et confirmer par `Y`

Compilé Klipper 

    make clean
	make
	
Identification du périphérique CAN afin de permettre de connaitre son ID pour le flasher.

    sudo service klipper stop
	cd ~/klipper/lib/canboot/
	python3 flash_can.py -q

Un numéro UUID apparait. Veuillez copier ce numéro
<center><img src="Images\CanBus 10.png"></center>

Flasher votre carte (veuillez remplacer `4b928e40f4ba` par votre numéro identifié)

    python3 flash_can.py -i can0 -f ~/klipper/out/klipper.bin -u 4b928e40f4ba
	sudo service klipper start

Merci [Chripink](https://github.com/chripink/CanBus-Tuto) pour son incroyable tuto dont je me suis inspiré

<hr>

### XXXXX

<hr>

### XXXXX

<hr>

### XXXXX

<hr>

### XXXXX

<hr>

### XXXXX

<hr>

### XXXXX

<hr>

### Fichiers customisés

Intégrer mes fichiers suivants :
* Répertoire `_MaConfig`
* fichier `override.cfg`

<hr>