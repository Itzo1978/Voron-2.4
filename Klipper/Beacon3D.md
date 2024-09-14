# Installation Beacon3D
* [Documentation Beacon 3D (en anglais)](https://docs.beacon3d.com/)

Code permettant de modifier la valeur 'TRSYNC_TIMEOUT' dans le fichier 'mcu.py'

## Install Beacon Klipper Module¶
Clonez `beacon_klipper` depuis git et exécutez le script d'installation :

    cd ~
    git clone https://github.com/beacon3d/beacon_klipper.git
    ./beacon_klipper/install.sh
	
## Configuration Moonraker Update Manager (en option)¶
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


# Calibration du Beacon3D

Faites un homing des axes X and Y:

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