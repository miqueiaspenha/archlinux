#### Configurações Iniciais do Arch Linux

Mudar o nome do hostname
```
sudo hostnamectl set-hostname nome-do-seu-pc
```

Reiniciar o sistema
```
sudo reboot
```

Para configurar sua rede indico que faça via cabo depois na parte gráfica faça a configuração da sua wifi
Verificar qual sua interface de rede
```
sudo ip link ou sudo ifconfig
```

Se aparecer somente o lo, execute
```
sudo dhcpcd
```

Ativar o dhcp para iniciar com rede cabeada ativada
```
sudo systemctl enable dhcpcd@minha-interface-de-rede
```

Reiniciar a o sistema
```
sudo reboot
```

Instalando pacotes para o som
```
pacman -S alsa-utils
```

Testando as configurações
Obs.: Talvez seja preciso aperta F6 e escolher o seu dispositivo de som
```
sudo alsamixer
```

Instalar o xorg e outros pacotes para o sistema fique avontade para ler sobre eles instalar o vc acha necessário
```
sudo pacman -S xorg-server xorg-xinit xorg-server-utils mesa ttf-dejavu samba smbclient networkmanager networkmanager-vpnc networkmanager-pptp networkmanager-openconnect network-manager-applet gvfs gvfs-smb sshfs
```

Escolha o seu driver de video e o instale com o pacman

```
sudo pacman -S o-meu-driver-de-video-abaixo
```

Drivers

```
virtualbox-guest-utils - para o virtualbox
```

```
nvidia - para placas nvidia
```

```
xf86-video-ati - para placas amd-radeon
```

```
xf86-video-intel - para drivers da intel
```

Ativar o gerenciador de rede automaticamente
```
sudo systemctl enable NetworkManager
```

Para a interface gráfica usei o Gnome, mas existe várias outras que vc deveria conhecer como XFCE4, KDE LXDE... e por ai vai.

Instalando o Gnome completo
```
sudo pacman -S gnome gnome-extra
```

Para inicializar o Gnome edite o arquivo
```
sudo nano ~/.xinitrc
```

Comente essas linhas se a caso existirem
```
#twm &
#xclock -geometry 50x50-1+1 &
#xterm -geometry 80x50+494+51 &
#xterm -geometry 80x20+494-0 &
#exec xterm -geometry 80x66+0+0 -name login
```

Adcione essa
```
exec gnome-session
```

Para subir sempre o GDM ao inicalizar o sistem
```
sudo systemctl enable gdm
```

Instalando o conjunto de ícones do faenza depois mude no Gnome Tweak
```
yaourt -S faenza-icon-theme
```

reinicie o systema
```
sudo reboot



Indico fazer tbm a intalação do Yaourt

[Instalando o Yaourt no Arch Linux](https://github.com/miqueiaspenha/instalacao-archlinux/blob/master/instalando-yaourt-archlinux.md)