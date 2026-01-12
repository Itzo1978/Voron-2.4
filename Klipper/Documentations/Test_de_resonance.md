# TEST DE RESONANCE

## GCODE à lancer 

## Axe X

    TEST_RESONANCES AXIS=X

## Axe Y

    TEST_RESONANCES AXIS=Y


à insérer dans le SSH pour générer le résultat final en image`

    ~/klipper/scripts/calibrate_shaper.py /tmp/resonances_x_*.csv -o /tmp/shaper_calibrate_x.png
    ~/klipper/scripts/calibrate_shaper.py /tmp/resonances_y_*.csv -o /tmp/shaper_calibrate_y.png
