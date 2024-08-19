# WEBCAM
* [Crowsnest](https://crowsnest.mainsail.xyz/)

### Installation 

    cd ~
    git clone https://github.com/mainsail-crew/crowsnest.git
    cd ~/crowsnest
    sudo make install

A insérer dans `moonraker.conf`

    [update_manager crowsnest]
    type: git_repo
    path: ~/crowsnest
    origin: https://github.com/mainsail-crew/crowsnest.git
    install_script: tools/pkglist.sh