# Configurando Bluetooth no Arch Linux

```
sudo pacman -S blueman
sudo pacman -S bluez
sudo pacman -S bluez-hid2hci
sudo systemctl enable bluetooth.service
sudo systemctl start bluetooth.service
sudo reboot
``` 
 
Testando no bluetooth BCM2046. 

Comando para lista o seu dispositivo 

lsusb 
