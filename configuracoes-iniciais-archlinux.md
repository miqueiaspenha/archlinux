#### Configurações Iniciais do Arch Linux

```
#Mudar o nome do hostname
sudo hostnamectl set-hostname nome-do-seu-pc
#Reiniciar o sistema
sudo reboot
```

```
Para configurar sua rede indico que faça via cabo depois na parte gráfica faça a configuração da sua wifi
#Verificar qual sua interface de rede
ip link ou  ifconfig
#Se aparecer somente o lo, execute
sudo dhcpcd
#Ativar o dhcp para iniciar com rede cabeada ativada
sudo systemctl enable dhcpcd@minha-interface-de-rede
Reiniciar a o sistema
reboot
```

```
#setar o som
pacman -S alsa-utils
#testando as configurações
#Obs.: Talvez senha preciso aperta F6 e escolher o seu dispositivo de som
alsamixer
```

```
#Instalar o xorg e outros pacotes para o sistema
sudo pacman -S xorg-server xorg-xinit xorg-server-utils mesa ttf-dejavu samba smbclient networkmanager networkmanager-vpnc networkmanager-pptp networkmanager-openconnect network-manager-applet gvfs gvfs-smb sshfs
#escolher as
```

```
#instalar driver de video
#Instalar o que for o seu:
virtualbox-guest-utils - para o virtualbox
nvidia - para placas nvidia
xf86-video-ati - para placas amd-radeon
xf86-video-intel - para drivers da intel
```

```
#Instalando o conjunto de ícones do faenza
yaourt -S faenza-icon-theme
```

```
#esse foi o que usei, substitua pelo seu respectivo:
sudo pacman -S virtualbox-guest-utils
#instalar os opcionais - se quiser, claro
```

```
#ativar o gerenciador de rede automaticamente
sudo systemctl enable NetworkManager
```