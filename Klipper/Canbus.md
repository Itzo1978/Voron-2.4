# CANBUS
* [Guide d'installation du Canbus](https://github.com/chripink/CanBus-Tuto)

Code permettant de modifier la valeur 'TRSYNC_TIMEOUT' dans le fichier 'mcu.py'

    cp /home/pi/klipper/klippy/mcu.py /home/pi/klipper/klippy/mcu.py.bak && sed -i 's/TRSYNC_TIMEOUT = 0.025/TRSYNC_TIMEOUT = 0.05/' /home/pi/klipper/klippy/mcu.py && sudo systemctl restart klipper