# instalacao-archlinux

## Procedimento para instalação do Arch Linux

```
#Efetua a mudança do teclado para BR ABNT 2
loadkeys br-abnt2
```

```
#Alterando a fonte para uma melhor visualização
setfont Lat2-Terminus16
```

```
#Se quiser voltar a font defaultload
setfont
```

```
#Alterar o arquivo locade.gen de descomentar as duas linha referente a PT-BR
nano /etc/locale.gen

#Comentar a linha
#en_US.UTF-8 UTF-8

#Descomentar essa linha
pt_BR.UTF-8 UTF-8
pt_BR ISO-8859-1
```

```
#Gerar o idioma
locale-gen
```

```
#Exportar para a instalação atual
export LANG=pt_BR.UTF-8
```

```
#Verificando interface de rede
iwconfig
```

```
#Configurando a conexão cabeada
systemctl start dhcpcd
```

```
#Caso esteja usando Wifi executar e conectar na sua rede
#Caso ocorra algum erro ligue de desligue seu adaptador Wifi com o teclado
wifi-menu nome-da-interface
```

```
#Testando a conexão com a internet
ping -c 3 www.google.com
```

```
#Visualizar as partições
fdisk -l
```

```
#Obtive um problema com a label DOS e tive que colocar como GPT segue o comando
#Obs.: Se vc tiver usando maquina virtual escolha GPT
parted /dev/sda mklabel gpt mkpart P1 ext3 1MiB 8MiB
```

```
#Fazendo o particionamento
cfdisk /dev/sda
#se for GPT criar uma particao de boot BIOS de 2Mb
```

```
#Com Label GPT
#Alguns valores dependem do seu HD e memoria RAM
/dev/sda1 - Normalmente se cria uma particao (2M) - Bios Inicializacao
/dev/sda2 - Uma particao para / [root] (Depende do HD) - Sistema de arquivos Linux
/dev/sda3 - Uma particao SWAP (O dobro da sua RAM) - SWAP
/dev/sda4 - Uma particao para /home [home] (Normalmente maior que a do root) - Sistema de arquivos Linux
```

```
#Fortama as partições para ext4
mkfs.ext4 /dev/sda2
mkfs.ext4 /dev/sda4
```

```
#Formata a particão para swap e liga
mkswap /dev/sda3
swapon /dev/sda3
```

```
#Ver todas as partições
lsblk /dev/sda
```

```
#montar a particao de / [root] para /mnt
mount /dev/sda2 /mnt
```

```
#Criar a pasta home e montar a particao na /mnt/home [/home]
mkdir /mnt/home
mount /dev/sda4 /mnt/home
```

```
#Instalando o sistema base
pacstrap /mnt base base-devel
```

```
#Fazer a geração do fstab  que descreve as partições
genfstab -U -p /mnt >> /mnt/etc/fstab
```

```
#Ver o que esta escrito nesse arquivo
cat /mnt/etc/fstab
```

```
#Faz o login no ambiente de instalação para instalar/configurar itens necessários
arch-chroot /mnt
```

```
#Efetua a mudança da liguagem da sua instalação
#Alterar o arquivo locade.gen de descomentar as duas linha referente a PT-BR
#Descomentar essas linhas
pt_BR.UTF-8 UTF-8
pt_BR ISO-8859-1
```

```
#Gerar o idioma
locale-gen
```

```
Criar o aquirvo de configuração de linguagem
echo LANG=pt_BR.UTF-8 > /etc/locale.conf
```

```
#Exportar para a instalação atual
export LANG=pt_BR.UTF-8
```

```
#Alterar o arquivo para guardar configuracoes de teclado para persistir depois do reboot
nano /etc/vconsole.conf
#Adicionar essas configuracoes no arquivo
KEYMAP=br-abnt2
FONT=Lat2-Terminus16
FONT_MAP=
```

```
#Configurando o fusohorario
#Ver todos os timezones disponiveis procure o nome da sua Cidade ou uma de mesmo fusohorario da sua
ls /usr/share/zoneinfo/America
#Setar a configuracao do timezone da instalação para Fortaleza
ln -s /usr/share/zoneinfo/America/Fortaleza /etc/localtime
```

```
#Comando para sincronizar o relogio do hardware com o do sistema
hwclock --systohc --utc
#Verifica o horário
date
```

```
#Configurando a rede cabeada
systemctl enable dhcpcd@eth0.service
```

```
#Configurando a rede wireless
pacman -S wireless_tools wpa_supplicant wpa_actiond netcf dialog
```

```
#Comando para não perder a configuração da wireless depois do reboot
systemctl enable net-auto-wireless.service
```

```
#Habilitar repositorios do pacman 32 e 64 bits
#descomentar multilib
nano /etc/pacman.conf
```

```
#Fazer a sincronização dos repositorios
#Para qualquer configuracao efetuada no pacman executar esse comando
pacman -Sy
```

```
#Alterar a senha de root
passwd
```

```
#Criando o usuario, definindo os grupos, e alterando a senha
useradd -m -g users -G wheel,storage,power -s /bin/bash meu-usuario
passwd meu-usuario
```

```
#Executar a instalação do sudo se não existir
pacman -S sudo
```

```
#editar os propriedades de sudo
#descomentar a linha que mostra wheel
EDITOR=nano visudo
```

```
#Adiconar seu usuario como root de root ALL=(ALL) ALL
seu-usuario ALL=(ALL) ALL
```

```
#Instlando ferramentas para rede
pacman -S net-tools
```

```
#Baixar grub para a instalação
pacman -S grub-bios
#Instalar grub
grub-install --target=i386-pc --recheck /dev/sda
#Copia o arquivo de configuração do grub
cp /usr/share/locale/en\@quot/LC_MESSAGES/g­rub.mo /boot/grub/locale/en.mo
#Criar o arquivo de configuracao do grub
grub-mkconfig -o /boot/grub/grub.cfg
```

```
#Saindo do arch-chroot
exit
```

```
#Semonstando as particoes
umount /mnt/home
umount /mnt
```

```
#Reiniciar o sistema
reboot
```

#### VAMOS CONFIGURAR O ARCHLINUX

```
#Mudar o nome do hostname
sudo hostnamectl set-hostname nome-do-seu-pc
#Reiniciar o sistema
sudo reboot
```

```
#Verificar qual sua interface de rede
ip link ou  ifconfig
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
