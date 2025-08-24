# CANBUS
	
Les informations suivantes sont reprises du site [Esoterical](https://canbus.esoterical.online/). A suivre pas à pas les indications :

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

## Flashage de la carte mère en mode USB CAN Bridge avec KATAPULT

Eteindre votre imprimante et insérer un jumper (en violet) sur la carte mère pour le passer en mode DFU
<center><img src="..\Images\DFU_octopus.png"></center>

Il faut installer certaines dépendances :

    sudo apt update
    sudo apt upgrade
    sudo apt install python3 python3-serial

Installez KATAPULT

    test -e ~/katapult && (cd ~/katapult && git pull) || (cd ~ && git clone https://github.com/Arksine/katapult) ; cd ~

Configurez KATAPULT selon cette image
<center><img src="..\Images\Katapult Config.png"></center>

    cd ~/katapult
    make menuconfig

Confilez le firmware

    make clean
    make

Vérifier si la carte est bien en mode DFU

    lsusb
	
<center><img src="..\Images\confirmation DFU_octopus.png"></center>

Si ce n'est pas le cas, appuyer sur le bouton RESET (en vert sur la vue représentant la carte mère)

Si c'est le cas

    cd ~/katapult
    make
    sudo dfu-util -R -a 0 -s 0x08000000:mass-erase:force:leave -D ~/katapult/out/katapult.bin -d 0483:df11

Retirez le jumper et faire un reset. Vérifier si la carte est identifiée en KATAPULT

    ls /dev/serial/by-id

Je devrais voir `/dev/serial/by-id/usb-katapult_stm32f446xx_260056001251373234333632-if00`
<center><img src="..\Images\octopus_identifie_katapult.png"></center>

> [!WARNING]
> Si ce n'est pas identifié, [recommencer la procédure](https://github.com/Itzo1978/Voron-2.4/blob/main/Klipper/Check-list_v2.3941.md#flashage-de-la-carte-m%C3%A8re-en-mode-usb-can-bridge-avec-katapult)

<hr>

## Flashage de la carte mère en mode USB CAN Bridge avec KLIPPER

Configurez KLIPPER selon cette image
<center><img src="..\Images\Klipper USB-CAN-Bridge Config.png"></center>

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

<center><img src="..\Images\Geschwister Schneider CAN adapter.png"></center>

Vérifiez que l'interface can0 est active :

    ip -s -d link show can0

<center><img src="..\Images\can0 active.png"></center>

Lancer la requête pour récupérer le canbus_uuid de votre carte mère :

    ~/klippy-env/bin/python ~/klipper/scripts/canbus_query.py can0
	
Je devrais voir `Detected UUID: 147d4dc27d4a, Application: Klipper`

Relancer KLIPPER

    sudo service klipper start

<hr>

## Flashage de la Toolhead avec KATAPULT

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
<center><img src="..\Images\Katapult Config Toolhead.png"></center>

    cd ~/katapult
    make menuconfig

Confilez le firmware

    make clean
    make

Vérifier si la Toolhead est bien en mode DFU

    lsusb
	
<center><img src="..\Images\confirmation DFU_toolhead.png"></center>

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

> [!WARNING]
> Il est important que le 2ème UUID (`1469b906a561`) doit être en KATAPULT et non en KLIPPER. Si ce n'est pas le cas, [recommencer la procédure](https://github.com/Itzo1978/Voron-2.4/blob/main/Klipper/Check-list_v2.3941.md#flashage-de-la-toolhead-avec-katapult).

<hr>

## Flashage de la Toolhead avec KLIPPER

Configurez KLIPPER selon cette image
<center><img src="..\Images\Klipper Toolhead Config.png"></center>

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

<center><img src="..\Images\UUID.png"></center>

Ces deux UUID sont à utiliser dans la section [mcu] du fichier de configuration de l'imprimante

Redémarrez le service Klipper

    sudo service klipper start
	
<hr>

# Autre astuces
* [Guide d'installation du Canbus](https://github.com/chripink/CanBus-Tuto)

Code permettant de modifier la valeur 'TRSYNC_TIMEOUT' dans le fichier 'mcu.py'

    cp /home/pi/klipper/klippy/mcu.py /home/pi/klipper/klippy/mcu.py.bak && sed -i 's/TRSYNC_TIMEOUT = 0.025/TRSYNC_TIMEOUT = 0.05/' /home/pi/klipper/klippy/mcu.py && sudo systemctl restart klipper