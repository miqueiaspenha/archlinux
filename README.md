# Instalacão do Archlinux

![Arch Linux](http://danlynch.org/wp-content/uploads/2009/03/arch-linux-logo.png)

## Procedimento para instalação do Arch Linux

Obs.: O # na frente dos comandos para quem não sabe indica que vc está como root do sistema.

Efetue o download da iso aqui
[https://www.archlinux.org/download/](https://www.archlinux.org/download/)

Pegue um pendrive vazio, sem dados.

Para criar o disco de inicialização no Windows usei o Ruffus
[https://rufus.akeo.ie/?locale=pt_BR](https://rufus.akeo.ie/?locale=pt_BR)

No linux usei o comando dd
```
# dd if=iso-do-archlinux.iso of=/dev/sdX
```
O X é referente ao seu pendrive, exemplo: /dev/sdc, para saber qual é o pendrive vc pode usar o comando
```
# fdisk -l
```

Coloque seu pc para efeutar o boot pelo o pendrive, alguns computadores precisam de algumas configurações na bios no meu caso um F12 foi o suficiente para efetuar o boot.

Com o computador inicializado com o Arch Linux como root # execute os comandos a baixo:

Efetua a mudança do teclado para BR ABNT 2
```
# loadkeys br-abnt2
```

Alterando a fonte para uma melhor visualização
```
# setfont Lat2-Terminus16
```

Se quiser voltar a font defaultload
```
# setfont
```

Alterar o arquivo locale.gen
```
#nano /etc/locale.gen
```

Comentar a linha
```
#en_US.UTF-8 UTF-8
```

Descomentar essas linhas
```
pt_BR.UTF-8 UTF-8
pt_BR ISO-8859-1
```

Gerar o idioma
```
# locale-gen
```

Exportar alterar o idioma da instalação atual
```
export LANG=pt_BR.UTF-8
```

Verificando suas interfaces de rede
```
# iwconfig
```

Configurando a conexão cabeada
```
# systemctl start dhcpcd
```

Caso esteja usando Wifi executar e conectar na sua rede
Caso ocorra algum erro ligue de desligue seu adaptador Wifi com o atalho fn do teclado
```
# wifi-menu nome-da-interface
```

Testando a conexão com a internet
```
# ping -c 3 www.google.com
```

Visualizar as partições
```
fdisk -l
```

Tive um problema com a label DOS se vc também tiver tente colocar como GPT segue o comando
Obs.: Se vc tiver usando maquina virtual escolha GPT
```
# parted /dev/sda mklabel gpt mkpart P1 ext3 1MiB 8MiB
```

Fazendo o particionamento onde sda é a sua partição principal
Obs.: Como escolhemos GPT vamos criar uma particao de boot BIOS de 2Mb(Eu coloco 5Mb)
```
cfdisk /dev/sda
```

Alguns valores dependem do seu HD e memoria RAM
```
/dev/sda1 - Normalmente se cria uma particao (2M) - Tipo: Bios Inicializacao
/dev/sda2 - Uma particao para / [root] (Aqui ficará seus programas se vc usa muitos, deixe um espaço bom) - Tipo: Sistema de arquivos Linux
/dev/sda3 - Uma particao SWAP (O total ou o dobro da sua RAM) - Tipo: SWAP
/dev/sda4 - Uma particao para /home [home] (Normalmente maior que a do root pq aqui ficará seu arquivos) - Tipo: Sistema de arquivos Linux
```

Fortamar as partições para ext4
```
# mkfs.ext4 /dev/sda2
# mkfs.ext4 /dev/sda4
```

Formatar a particão para swap e na sequência montando
```
# mkswap /dev/sda3
# swapon /dev/sda3
```

Ver todas as partições
```
# lsblk /dev/sda
```

Montar a particao de / [root] para /mnt
```
# mount /dev/sda2 /mnt
```

Criar a pasta home e montar a particao na /mnt/home [/home]
```
# mkdir /mnt/home
# mount /dev/sda4 /mnt/home
```

Instalando o sistema base
```
# pacstrap /mnt base base-devel
```

Fazer a geração do fstab que descreve as partições
Obs.: Não esquecer nunca
```
# genfstab -U -p /mnt >> /mnt/etc/fstab
```

Verificar o que esta escrito nesse arquivo
```
# cat /mnt/etc/fstab
```

Faz o login no ambiente de instalação para instalar/configurar itens necessários
```
# arch-chroot /mnt
```

Efetua a mudança da liguagem da sua instalação
Alterar o arquivo /etc/locale.gen com seu editor preferido de descomentar as duas linha referente a PT-BR
```
# nano /etc/locale.gen

pt_BR.UTF-8 UTF-8
pt_BR ISO-8859-1
```

Gerando o idioma
```
# locale-gen
```

Criar o arquivo de configuração de linguagem
```
# echo LANG=pt_BR.UTF-8 > /etc/locale.conf
```

Mudar o idioma da instalação atual
```
# export LANG=pt_BR.UTF-8
```

Alterar o arquivo para guardar configuracoes de teclado para persistir depois do reboot
```
# nano /etc/vconsole.conf
```

Adicionar essas configuracoes no arquivo
```
KEYMAP=br-abnt2
FONT=Lat2-Terminus16
FONT_MAP=
```

Configurando o fusohorario

Ver todos os timezones disponiveis, procure o fusohorario desejado
```
# ls /usr/share/zoneinfo/America
```

Guarda a configuracao do timezone
```
# ln -s /usr/share/zoneinfo/America/Fortaleza /etc/localtime
```

Comando para sincronizar o relogio do hardware com o do sistema
```
# hwclock --systohc --utc
```

Verifica o horário
```
# date
```

Instalando as ferramentas de rede
```
# pacman -S net-tools
```

Verificando suas interfaces de rede
```
# ifconfig
```

Configurando a rede cabeada com sua interface
```
# systemctl enable dhcpcd@sua-inetrface

Instalando pacotes para usar a rede wireless
```
# pacman -S wireless_tools wpa_supplicant wpa_actiond netcf dialog
```

Comando para não perder a configuração da wireless depois do reboot
```
# systemctl enable net-auto-wireless.service
```

Habilitar repositorios do pacman 32 e 64 bits

Editar o arquivo /etc/pacman.conf
```
# nano /etc/pacman.conf
```

Descomentar multilib
```
[multilib]
Include = /etc/pacman.d/mirrorlist
```

Fazer a sincronização dos repositorios
Obs.: Para qualquer configuracao efetuada no pacman executar esse comando, server também para atualizar o sistema
```
# pacman -Syu
```

Alterar a senha de root
```
# passwd
```

Criando o usuario, definindo os grupos onde ele deve pertencer
```
# useradd -m -g users -G wheel,storage,power -s /bin/bash meu-usuario
```

```
Alterando a senha do seu usuario
# passwd meu-usuario
```

Instala o sudo se ele não existir
```
# pacman -S sudo
```

Editar os propriedades de sudo
```
# nano /etc/sudoers
```

Adiconar seu usuario como root depois de root ALL=(ALL) ALL
```
seu-usuario ALL=(ALL) ALL
```

Criando um ambi9ente ramdisk inicial
```
# mkinitcpio -p linux
```

Baixando o grub para a instalação
```
# pacman -S grub-bios
```

Instalando o grub
```
# grub-install --target=i386-pc --recheck /dev/sda
```

Copia o arquivo de configuração do grub
```
# cp /usr/share/locale/en\@quot/LC_MESSAGES/g­rub.mo /boot/grub/locale/en.mo
```

Criar o arquivo de configuracao do grub
```
# grub-mkconfig -o /boot/grub/grub.cfg
```

Autocomplete no sudo usando tecla tab

Abrir o arquivo .bashrc
```
# nano /home/seu-usuario/.bashrc
```

Adicone no final do arquivo
```
complete -cf sudo
```

Para atualizar o console
```
# export /home/seu-usuario/.bashrc
```

agora teste algum comando com o tab

Mudar o nome do hostname
```
# hostnamectl set-hostname nome-do-seu-pc
```

feche e salve e abra o terminal e teste

Saindo do arch-chroot
```
# exit
```

Demonstando as particoes
```
# umount /mnt/home
# umount /mnt
```

Reiniciar o sistema
```
reboot
```

Parabéns agora vc tem o Arch Linux instalado mas ainda está nú... no próximos arquivos vc verá como instalar a parte gráfica e instalar alguns programas que pra mim são necessários.

[Configurações iniciais](https://github.com/miqueiaspenha/instalacao-archlinux/blob/master/configuracoes-iniciais-archlinux.md)

Qualquer dúvida estou a disposição no email: [miqueiaspenha@gmail.com](mailto:miqueiaspenha@gmail.com)

Faça um fork e me ajude a manter esse guia sempre atualizado.

Todas as informações foram extraídas do guia original diponibilizado no site do Arch Linux que é o melhor lugar para se ter as mais atualizadas informações sobre ele.

[https://wiki.archlinux.org/index.php/Installation_guide](https://wiki.archlinux.org/index.php/Installation_guide)

Abraços e boa sorte com essa linda distro!