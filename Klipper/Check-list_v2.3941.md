# Check-list pour une bonne installation

## Installation Pi Imager
* __Modèle de Raspberry :__ selon votre modèle
* __Système d'exploitation :__ voir images ci-dessous

<center><img src="Images\Raspberry Pi Imager 1.png"></center><br>
<center><img src="Images\Raspberry Pi Imager 2.png"></center>
<hr>

## Installation Klipper

Lancer l'application Putty pour piloter en SSH votre raspberry

### Mise à jour Kernel stable linux : 

    sudo apt-get update && sudo apt-get full-upgrade -y

<hr>

### Installation des différents outils : 

    sudo apt-get install git -y && sudo apt-get install dfu-util -y


<hr>

### Installation de [Kiauh](https://github.com/dw-0/kiauh)

1. Etape 1 :
Pour télécharger ce script, il est nécessaire d'avoir installé git. Si vous n'avez pas installé git, ou si vous n'êtes pas sûr, exécutez la commande suivante :

    sudo apt-get update && sudo apt-get install git -y


2. Etape 2:
Une fois git installé, utilisez la commande suivante pour télécharger KIAUH dans votre répertoire personnel :

    cd ~ && git clone https://github.com/dw-0/kiauh.git


3. Etape 3:
Démarrer KIAUH en copiant la commande ci-dessous :

    ./kiauh/kiauh.sh


* Installation de Klipper (1), Moonraker (2), Mainsail (3) et Crowsnest (14)
* _En option : Spoolman (13) pour l'[ERCF](https://github.com/Enraged-Rabbit-Community/ERCF_v2)_

<center><img src="Images\kiauh.png"></center>

Rebooter votre Raspberry et redémarrer votre session

<center><img src="Images\putty 1.png"></center>

<hr>

<H1>A FAIRE</H1>

### Installation de [Klippain](https://github.com/Frix-x/klippain)
Intégrer mes fichiers suivants :
* Répertoire `_MaConfig`
* fichier `override.cfg`

<hr>

### Installation de Shake&Tune
Installez Shake&Tune en l'exécutant via SSH sur votre imprimante :

    wget -O - https://raw.githubusercontent.com/Frix-x/klippain-shaketune/main/install.sh | bash

Ensuite, ajoutez ce qui suit à votre fichier `printer.cfg` et redémarrez Klipper :

    [shaketune]
    # result_folder: ~/printer_data/config/ShakeTune_results
    #    The folder where the results will be stored. It will be created if it doesn't exist.
    # number_of_results_to_keep: 3
    #    The number of results to keep in the result_folder. The oldest results will
    #    be automatically deleted after each runs.
    # keep_raw_csv: False
    #    If True, the raw CSV files will be kept in the result_folder alongside the
    #    PNG graphs. If False, they will be deleted and only the graphs will be kept.    
    # show_macros_in_webui: True
    #    Mainsail and Fluidd doesn't create buttons for "system" macros that are not in the
    #    printer.cfg file. If you want to see the macros in the webui, set this to True.
    # timeout: 300
    #    The maximum time in seconds to let Shake&Tune process the CSV files and generate the graphs.

<hr>

### XXXXX

<hr>

### XXXXX

<hr>

### XXXXX

<hr>