#Instalando o Yaourt no Arch Linux

A instalação do Yaourt é feita adiconando ao seu pacman.conf o repositório "archlinuxfr".

```
#Abra o pacman.conf como seu editor eu usei o nano
sudo nano /etc/pacman.conf
```

```
#Coloque o repositório no final do arquivo:

[archlinuxfr]
SigLevel = Never
Server = http://repo.archlinux.fr/x86_64

#Salve e Feche o aquivo
```

```
#Instale o yaourt com o pacman
sudo pacman -Sy yaourt
```

```
#Depois instale seu pacotes com
sudo yaourt -S nome-do-seu-pacote
```