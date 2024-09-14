# [Klipper Auto Speed](https://github.com/Anonoei/klipper_auto_speed)

## Installation de Auto Speed :

    cd ~
	git clone https://github.com/Anonoei/klipper_auto_speed.git
	cd klipper_auto_speed
	./install.sh

## Moonraker Update Manager :
	
	[update_manager klipper_auto_speed]
	type: git_repo
	path: ~/klipper_auto_speed
	origin: https://github.com/anonoei/klipper_auto_speed.git
	primary_branch: main
	install_script: install.sh
	managed_services: klipper

## Configuration

Insérer `[auto_speed]` dans le fichier `printer.cfg` ou `override.cfg` (utilisateur Klippain)

## Macro

