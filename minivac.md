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
- pulseaudio, pulseaudio-utils, pulseaudio-module-jack (son)
- pavucontrol (GUI) ou  pamix (shell) (mixer)
- ibus, ibus-anthy, fonts-vlgothic, fonts-ipafonts (jp)

## Applications

- lxterminal (pourri mais super leger)
- git
- netsurf-gtk (pourri mais supra leger)
- micro (super mais super leger)
- chromium (finalement un des plus leger/rapide)
- carbonyl pour naviguer dans le terminal

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

### son
- on rajoute pulseaudio (alsa seul trop limite) et pamixer pour controler depuis un shell
- dans pamixer s pour changer de sortie (jack<->hdmi) et les fleches G/D pour le volume
- pavuaudio pour une version gui de pamixer

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
- dans profile je disable "show title bar", background sur transparent

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
- de pref par ssh avec une adress en git@github.com:owner/repo pour eviter la reauthentification
- generer un token classic dans github
- ensuite c'est git add * / git commit -m "msg" / git push -u origin main

### Japonais
- ajouter ibus et les polices (cf plus haut)
- ajouter la locale jp 
`sudo dpkg-reconfigure locales`
- si ca marche pas avec dpkg tripatouiller dans etc/local-gen 
- locales -a affiche les locales dispo, verifier si l'ajout a marche
- sinon localectl aussi est utile notamment pour list et set
`localectl list-locales`
`localectl set-locale LANG=en_GB.UTF-8`
- parametrer ibus 
`ibus-setup`
- demmarage auto, rajouter dans ~/.bashrc
```
# Ibus start with openbox
export GTK_IM_MODULE=ibus
export XMODIFIERS=@im=ibus
export QT_IM_MODULE=ibus
```


## To do
- setup lightdm ? (la je log direct)





