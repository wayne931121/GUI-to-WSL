# Adding a GUI to Windows Subsystem for Linux

**WARNING!!** - Windows 10/11 PRO is Needed to Run this

1. Installing XFCE Desktop Environment
     - `sudo apt install -y xrdp xfce4 xfce4-goodies`
  
2. Configuring the Virtual Server
```sh
sudo cp /etc/xrdp/xrdp.ini /etc/xrdp/xrdp.ini.bak
sudo sed -i 's/3389/3390/g' /etc/xrdp/xrdp.ini
sudo sed -i 's/max_bpp=32/#max_bpp=32\nmax_bpp=128/g' /etc/xrdp/xrdp.ini
sudo sed -i 's/xserverbpp=24/#xserverbpp=24\nxserverbpp=128/g' /etc/xrdp/xrdp.ini
echo xfce4-session > ~/.xsession
```
4. Finish Setting up The RDP Access
     - `sudo nano /etc/xrdp/startwm.sh`
         -  Then comment out the following lines by placing a `#` before them: `test -x /etc/x11...` and the next line; `echo /bin/sh /etc/x11...`
         -  Now, add the following on a new line at the very end: `startxfce4`
         -  Save an Exit (ctrl+S & ctrl+X)

https://github.com/microsoft/WSL/issues/5719#issuecomment-1475679062

https://askubuntu.com/a/1324767/2364948
```sh
#!/bin/sh
# xrdp X session start script (c) 2015, 2017, 2021 mirabilos
# published under The MirOS Licence

# Rely on /etc/pam.d/xrdp-sesman using pam_env to load both
# /etc/environment and /etc/default/locale to initialise the
# locale and the user environment properly.

if test -r /etc/profile; then
        . /etc/profile
fi

unset DBUS_SESSION_BUS_ADDRESS
unset XDG_RUNTIME_DIR

#if test -r ~/.profile; then
#       . ~/.profile
#fi

#test -x /etc/X11/Xsession && exec /etc/X11/Xsession
#exec /bin/sh /etc/X11/Xsession

startxfce4
```
```
sudo systemctl restart xrdp
```
     - `sudo /etc/init.d/xrdp start`
     - Now, open Remote Desktop on your Windows host machine, and connect to `localhost:3390`
