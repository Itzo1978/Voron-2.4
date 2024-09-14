# Klipper TMC Autotune
* [Klipper TMC Autotune](https://github.com/andrewmcgr/klipper_tmc_autotune)

Installation de TMC Autotune :

    wget -O - https://raw.githubusercontent.com/andrewmcgr/klipper_tmc_autotune/main/install.sh | bash

Mes moteurs actuels:
* Moteur A/B

    [motor_constants omc-17hs19-2004s1]
    resistance: 1.4
    inductance: 0.003
    holding_torque: 0.59
    max_current: 2
    steps_per_revolution: 200

* Moteur Z/Z1/Z2/Z3

    [motor_constants omc-17hs19-2004s1]
    resistance: 1.4
    inductance: 0.003
    holding_torque: 0.59
    max_current: 2
    steps_per_revolution: 200

* Moteur Extrudeur (LGX Extuder)

    [motor_constants bondtech-42H025H-0704A-005]
	#Bondtech LGX https://www.bondtech.se/downloads/TDS/Bondtech-LGX-Motor-42H025H-0704-002.pdf
	resistance: 4.4
	inductance: 0.0055
	holding_torque: 0.16
	max_current: 0.7
	steps_per_revolution: 200
