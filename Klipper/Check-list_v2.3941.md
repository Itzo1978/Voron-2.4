# Check-list pour une bonne installation

## Installation Pi Imager
* __Modèle de Raspberry :__ selon votre modèle
* __Système d'exploitation :__ voir images ci-dessous

<center><img src="Images\Raspberry Pi Imager 1.png"></center><br>
<center><img src="Images\Raspberry Pi Imager 2.png"></center>
<hr>

## Installation Klipper

Après avoir insérer la carte SD dans votre rasperry, allumer votre imprimante et lancer l'application Putty pour piloter en SSH votre raspberry

### Mise à jour Kernel stable linux : 

    sudo apt-get update && sudo apt-get full-upgrade -y

<hr>

### Installation des différents outils : 

    sudo apt-get install git -y && sudo apt-get install dfu-util -y


<hr>

### Installation de [Kiauh](https://github.com/dw-0/kiauh)

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

### Installation de [Klippain](https://github.com/Frix-x/klippain)

Lancer la procédure d'installation

    wget -O - https://raw.githubusercontent.com/Frix-x/klippain/main/install.sh | bash

1. Confirmer le souhaite d'installer Klippain par yes `y`
2. Sélection et installation du MCU : `y`
3. Choisir votre carte mère : pour ma part `BTT_Octopus (21)`
4. Carte CanBus EBB42 v1.2 :  pour ma part `BTT EBB36-42 v1.2 (10)`
5. Carte MMU/ERCF :  pour ma part `BTT_MBB_CAN v1.0 (5)`

<hr>

### Installation de Shake&Tune
Installez Shake&Tune en l'exécutant via SSH sur votre imprimante :

    wget -O - https://raw.githubusercontent.com/Frix-x/klippain-shaketune/main/install.sh | bash

<hr>

## Installation CanBus EBB42

### Sur votre imprimante

Avant de commencer, assurez-vous que votre imprimante est éteinte.

1. Insérer la prise USB-C sur la carte EBB42 (ou similaire) et sur votre raspberry pour assurer une connexion
<center><img src="Images\CanBus 1.png"></center>

2. Insérer le jumper sur la carte
<center><img src="Images\CanBus 2.png"></center>

3. Appuyer sur le bouton DFU __en même temps__ que vous allumer votre imprimante
<center><img src="Images\CanBus 3.png"></center>

### Dans SSH (avec Putty ou autre)

1. Connectez-vous à votre Imprimante en SSH.
2. Installez DFU_UTIL avec la commande suivante :

    sudo apt install dfu-util -y

3. Assurez vous que votre carte est bien détectée en mode DFU et taper `lsusb` pour identifier notre carte
<center><img src="Images\CanBus 4.png"></center>

Répeter cette opération jusqu'à l'identification de la carte qui se nomme `0483:df11`

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

### XXXXX

<hr>

### Fichiers customisés

Intégrer mes fichiers suivants :
* Répertoire `_MaConfig`
* fichier `override.cfg`

<hr>