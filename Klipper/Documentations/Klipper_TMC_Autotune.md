# Klipper TMC Autotune
* [Klipper TMC Autotune](https://github.com/andrewmcgr/klipper_tmc_autotune)

Installation de TMC Autotune :

    wget -O - https://raw.githubusercontent.com/andrewmcgr/klipper_tmc_autotune/main/install.sh | bash

## Installation sur Klipper
	
    [autotune_tmc stepper_x]
	motor: omc-17hs19-2004s1
	
	[autotune_tmc stepper_y]
	motor: omc-17hs19-2004s1
	
	[autotune_tmc stepper_z]
	motor: omc-17hs19-2004s1
	
	[autotune_tmc stepper_z1]
	motor: omc-17hs19-2004s1
	
	[autotune_tmc stepper_z2]
	motor: omc-17hs19-2004s1
	
	[autotune_tmc stepper_z3]
	motor: omc-17hs19-2004s1
	
	[autotune_tmc extruder]
	motor: ldo-36sth20-1004ahg-9T


## Mes moteurs actuels:
### Moteur A/B

    [motor_constants omc-17hs19-2004s1]
    resistance: 1.4
    inductance: 0.003
    holding_torque: 0.59
    max_current: 2
    steps_per_revolution: 200

### Moteur Z/Z1/Z2/Z3

    [motor_constants omc-17hs19-2004s1]
    resistance: 1.4
    inductance: 0.003
    holding_torque: 0.59
    max_current: 2
    steps_per_revolution: 200

### Moteur Extrudeur (Galileo 2) 

    [motor_constants ldo-36sth20-1004ahg-9T]
    resistance: 2.4
    inductance: 0.0014
    holding_torque: 0.10
    max_current: 1.0
    steps_per_revolution: 200
