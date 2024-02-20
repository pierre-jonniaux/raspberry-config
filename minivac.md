# MINIVAC RAPSBERRY SYSTEM CONFIG

## Image

- raspberry debian os lite 64 gb with imager
- setup internet and login in imager before burning the sd

## Desktop

- sudo apt install --no-install-recommends xserver-xorg xinit x11-xserver-utils
- openbox (win manager)
- tint2 (taskbar)
- nitrogen (pour les wallpaper)
- lightdm (gestionnaire de session at startup)
- xcompmgr (transparence)

## Applications

- lxterminal (pourri mais super leger)
- git
- netsurf-gtk (pourri mais supra leger)
- micro (super mais super leger)
- chromium (finalement un des plus leger/rapide)

## Config

### Swap : augmention
Augmente la taille de la swap (mais va se servir dans la sd card donc plus lent)
```
# turn off (just after boot with not much running)
sudo dphys-swapfile swapoff
# dans dphys-swapfile modifier CONF_SWAPSIZE=100 (=512 p.ex.)
sudo nano /etc/dphys-swapfile
# delete the original swap file and recreate it to fit the newly defined size
sudo dphys-swapfile setup
# on rallume la swap
sudo dphys-swapfile swapon
sudo reboot
```

### Wifi
- raspi-config system>network pour rajouter d'autre wifi

### Path
Setup le path vers home/bin dans home/.bashrc
```
# set path to user private bin
if [ -d "$HOME/bin" ]; then
  PATH="$HOME/bin:$PATH"
fi
```

### Autostart : nitrogen - tint2 - tranparence
- creer un fichier .config/openbox/autostart
- le chmod -755
```
nitrogen --restore &
tint2 &
xcompmgr &
```

### rc.xml : window snap - launcher
Add window snapping with super key in .config/openbox/rc.xml dans <keyboard></keyboard>
```
  <keyboard>
    <!-- Keybindings for window snapping (added by me)-->
    <keybind key="W-Left">
      <action name="UnmaximizeFull"/>
      <action name="MaximizeVert"/>
      <action name="Raise"/>
      <action name="MoveResizeTo">
        <width>50%</width>
        <x>0</x>
        <y>0</y>
      </action>
    </keybind>
    <keybind key="W-Right">
      <action name="UnmaximizeFull"/>
      <action name="MaximizeVert"/>
      <action name="Raise"/>
      <action name="MoveResizeTo">
        <width>50%</width>
        <x>50%</x>
        <y>0</y>
      </action>
    </keybind>
    <!-- end of my modif-->
  </keyboard>  
```
Add launcher keybinding to add in <keyboard></keyboard> as well
```
<!-- Keybindings for running applications -->
    <keybind key="W-f">
      <action name="Execute">
          <name>Thunar</name>
        <command>thunar</command>
      </action>
    </keybind>
    <keybind key="W-e">
      <action name="Execute">
          <name>Micro</name>
        <command>terminator -e micro</command>
      </action>
    </keybind>
    <keybind key="W-t">
      <action name="Execute">
        <name>Terminator</name>
        <command>terminator</command>
      </action>
    </keybind>
    </keybind>
        <keybind key="W-w">
      <action name="Execute">
        <name>Netsurf</name>
        <command>netsurf</command>
      </action>
    </keybind>
```

### Terminator
- disable keybinding to ctrl-shift-arrow qui interfere avec la selection de texte dans micro

### Micro
- copie maline :ajoute la selection copie du mot en cours sur ctrl-space (Ctrl-e pour passer en commande)
`bind CtrlSpace WordRight&SelectWordLeft&Copy`
- ajoute le jump plugin, besoin de fzf et exuberant-ctags
`sudo apt-get install fzf exuberant-ctags`

### Git
- si elle n'existe pas cree un clef ssh
`ssh-keygen -t rsa -C "***@gmail.com"`
- la copier (sinon elle est dans ~/.ssh/id_rsa.pub)
- se connecter au compte github et rajouter la clef (>settings>sshkeys)

## To do
- setup lightdm ? (la je log direct)
