# BoxTurtle
	
Les informations suivantes sont reprises du site [Esoterical](https://canbus.esoterical.online/). A suivre pas à pas les indications :


## Flashage de la carte mère BoxTurtle en mode USB CAN Bridge avec KATAPULT

Il faut installer certaines dépendances :

    sudo apt update
    sudo apt upgrade
    sudo apt install python3 python3-serial

Installez KATAPULT

    test -e ~/katapult && (cd ~/katapult && git pull) || (cd ~ && git clone https://github.com/Arksine/katapult) ; cd ~

Configurez KATAPULT selon cette image
<center><img src="..\Images\Katapult Config BT.png"></center>

    cd ~/katapult
    make menuconfig

Confilez le firmware

    make clean
    make

Vérifier si la carte est bien en mode DFU

    lsusb
	
<center><img src="..\Images\confirmation DFU_octopus.png"></center>

Si ce n'est pas le cas, appuyer sur le bouton RESET et BOOT en même temps. Relacher RESET d'abord, et BOOT 5 secondes après

Si c'est le cas

    cd ~/katapult
    make
    sudo dfu-util -R -a 0 -s 0x08000000:mass-erase:force:leave -D ~/katapult/out/katapult.bin -d 0483:df11

Vérifier si la carte est identifiée en KATAPULT

    ls /dev/serial/by-id

Je devrais voir `/dev/serial/by-id/usb-katapult_stm32h723xx_180019000551333235393937-if00
<center><img src="..\Images\BT_identifie_katapult.png"></center>

> [!WARNING]
> Si ce n'est pas identifié, recommencer la procédure.

<hr>

## Flashage de la carte mère BoxTurtle en mode USB CAN Bridge avec KLIPPER

Configurez KLIPPER selon cette image
<center><img src="..\Images\Klipper BT USB-CAN-Bridge Config.png"></center>

    cd ~/klipper
	make menuconfig

Confilez le firmware

    make clean
    make

Utiliser KATAPULT pour flasher KLIPPER

    sudo service klipper stop

Lancer la commande pour flasher la carte mère 

    python3 ~/katapult/scripts/flashtool.py -f ~/klipper/out/klipper.bin -d /dev/serial/by-id/usb-katapult_stm32h723xx_180019000551333235393937-if00

Vérifier si la carte est identifiée en KATAPULT

> [!WARNING]
> Si on ne la voit pas tout de suite, éteindre l'imprimante et la rallumer

    ls /dev/serial/by-id

Je devrais voir `usb-Klipper_stm32h723xx_180019000551333235393937-if00`


>
serial: /dev/serial/by-id/usb-Klipper_stm32h723xx_180019000551333235393937-if00