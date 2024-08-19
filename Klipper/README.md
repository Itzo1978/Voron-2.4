# LISTE DES PLUGINS UTILISÉS SUR MA VORON V2.3941

## KLIPPER
* [Klippain](https://github.com/Frix-x/klippain)
* [Shake&Tune](https://github.com/Frix-x/klippain-shaketune)
	* [Documentation sur Shake&Tune](https://github.com/Frix-x/klippain-shaketune/blob/main/docs/README.md) 


### Installation de Shake&Tune
Installez Shake&Tune en l'exécutant via SSH sur votre imprimante :

    wget -O - https://raw.githubusercontent.com/Frix-x/klippain-shaketune/main/install.sh | bash

Ensuite, ajoutez ce qui suit à votre fichier printer.cfg et redémarrez Klipper :

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

## KIAUH
* [Kiauh](https://github.com/dw-0/kiauh)

### Installation de Kiauh :
1ère étape :

    sudo apt-get update && sudo apt-get install git -y

2ème étape :

    cd ~ && git clone https://github.com/dw-0/kiauh.git

3ème étape :

    ./kiauh/kiauh.sh

<hr>

## GESTION DES LEDS
* [Led Effect](https://github.com/julianschill/klipper-led_effect)

### Installation de Led Effect :

    cd ~
    git clone https://github.com/julianschill/klipper-led_effect.git
    cd klipper-led_effect
    ./install-led_effect.sh

<hr>
	
## CANBUS
Code permettant de modifier la valeur 'TRSYNC_TIMEOUT' dans le fichier 'mcu.py'

    cp /home/pi/klipper/klippy/mcu.py /home/pi/klipper/klippy/mcu.py.bak && sed -i 's/TRSYNC_TIMEOUT = 0.025/TRSYNC_TIMEOUT = 0.05/' /home/pi/klipper/klippy/mcu.py && sudo systemctl restart klipper

<hr>
	
## WEBCAM
* [Crowsnest](https://crowsnest.mainsail.xyz/)

### Installation 

    cd ~
    git clone https://github.com/mainsail-crew/crowsnest.git
    cd ~/crowsnest
    sudo make install

A insérer dans `moonraker.conf`

    [update_manager crowsnest]
    type: git_repo
    path: ~/crowsnest
    origin: https://github.com/mainsail-crew/crowsnest.git
    install_script: tools/pkglist.sh

<hr>

## KLIPPERSCREEN
* [Klipperscreen](https://github.com/bigtreetech/KlipperScreen)
* [Touchscreen Setup for Klipper](https://docs.ldomotors.com/en/guides/btt_43_rotate_guide)
