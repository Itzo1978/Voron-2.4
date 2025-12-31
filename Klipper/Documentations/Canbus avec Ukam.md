# CANBUS (avec [UKAM](https://github.com/fbeauKmi/update_klipper_and_mcus))

Avant de commencer, Kiauh doit être installé avec :
  [X] Klipper installé
  [X] Moonraker installé
  [X] Mainsail installé


## Préparation de l'installation du CANBUS

Assurez-vous que le service systemd-networkd est activé :

    sudo systemctl enable systemd-networkd


Démarrez le service :

    sudo systemctl start systemd-networkd


Vérifiez qu'il fonctionne correctement :

    systemctl | grep systemd-networkd


Assurez-vous qu'il s'affiche comme « loaded active running ».

<center><img src="..\Images\loaded active running.png"></center>


Désactiver complètement le service wait-online :

    sudo systemctl disable systemd-networkd-wait-online.service


Configurez « txqueuelen » pour l'interface :

    echo -e 'SUBSYSTEM=="net", ACTION=="change|add", KERNEL=="can*"  ATTR{tx_queue_len}="128"' | sudo tee /etc/udev/rules.d/10-can.rules > /dev/null


Vérifier si l'application s'est correctement exécutée

    cat /etc/udev/rules.d/10-can.rules


Cela devrait ressembler à ceci :
<center><img src="..\Images\10-can rules.png"></center>


Enfin, pour activer l'interface can0 et définir la vitesse, exécutez la commande suivante :

    echo -e "[Match]\nName=can*\n\n[CAN]\nBitRate=1M\nRestartSec=0.1s\n\n[Link]\nRequiredForOnline=no" | sudo tee /etc/systemd/network/25-can.network > /dev/null


Vérifier si l'application s'est correctement exécutée

    cat /etc/systemd/network/25-can.network


Cela devrait ressembler à ceci :
<center><img src="..\Images\25-can network.png"></center>


Rebootez votre Raspberry

    sudo reboot now

<hr>

## Installation Katapult

Il faut installer certaines dépendances :

    sudo apt update
    sudo apt upgrade
    sudo apt install python3 python3-serial


Installez KATAPULT

    test -e ~/katapult && (cd ~/katapult && git pull) || (cd ~ && git clone https://github.com/Arksine/katapult) ; cd ~


## Installation d'UKAM

Installer UKAM

    cd ~
    git clone https://github.com/fbeauKmi/update_klipper_and_mcus.git ukam


Avec Filezilla, insérer le fichier 'mcus.ini' dans le répertoire '/home/pi/printer_data/config/ukam/'


Lancer Ukam

    ./ukam/ukam.sh


Configurez KLIPPER selon cette image
<center><img src="..\Images\Klipper USB-CAN-Bridge Config.png"></center>









# Autre astuces
* [Guide d'installation du Canbus](https://github.com/chripink/CanBus-Tuto)

Code permettant de modifier la valeur 'TRSYNC_TIMEOUT' dans le fichier 'mcu.py'

    cp /home/pi/klipper/klippy/mcu.py /home/pi/klipper/klippy/mcu.py.bak && sed -i 's/TRSYNC_TIMEOUT = 0.025/TRSYNC_TIMEOUT = 0.05/' /home/pi/klipper/klippy/mcu.py && sudo systemctl restart klipper