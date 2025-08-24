# Check-list pour une bonne installation

## MA CONFIGURATION

* Raspberry PI5
* Carte mère BTT Octopus
* EBB42 v1.2
* Beacon3D
* Hotend TZ-V6
* StealthMax
* BoxTurtle

<hr>

## INSTALLATION AVEC RASPBERRY PI IMAGER
* __Modèle de Raspberry :__ selon votre modèle
* __Système d'exploitation :__ voir images ci-dessous

<center><img src="Images\Raspberry Pi Imager 1.png"></center><br>
<center><img src="Images\Raspberry Pi Imager 2.png"></center>
<hr>

## CHECKLIST D'INSTALLATION

Après avoir insérer la carte SD dans votre rasperry, allumer votre imprimante et lancer l'application [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) ou [MobaXterm](https://mobaxterm.mobatek.net/download.html) pour piloter en SSH votre raspberry

### Mise à jour Kernel stable linux : 

    sudo apt-get update && sudo apt-get full-upgrade -y

<hr>

### 	Installation des différents outils : 

    sudo apt-get install git -y && sudo apt-get install dfu-util -y

<hr>

### Installation des modules dans l'ordre :

- [ ] [Kiauh](https://github.com/Itzo1978/Voron-2.4/blob/main/Klipper/Documentations/Kiauh.md)
- [ ] [CANBUS](https://github.com/Itzo1978/Voron-2.4/blob/main/Klipper/Documentations/Canbus.md)
- [ ] [Klippain et Shake&Tune](https://github.com/Itzo1978/Voron-2.4/blob/main/Klipper/Documentations/Klippain.md)
- [ ] [Gestion des LEDs](https://github.com/Itzo1978/Voron-2.4/blob/main/Klipper/Documentations/Gestion_des_LEDS.md)
- [ ] [TMC Auto Tune](https://github.com/Itzo1978/Voron-2.4/blob/main/Klipper/Documentations/Klipper_TMC_Autotune.md)
- [ ] [Beacon 3D](https://github.com/Itzo1978/Voron-2.4/blob/main/Klipper/Documentations/Beacon3D.md)
- [ ] [Blobifier]()
- [ ] [StealthMax]()
- [ ] [BoxTurtle]()


### Fichiers customisés

[Intégrer mes fichiers suivants](https://github.com/Itzo1978/Voron-2.4/tree/main/Klipper/Config%20Klipper)

_MaConfig
mcu.cfg
moonraker.conf
overrides.cfg
printer.cfg
variables.cfg

<hr>