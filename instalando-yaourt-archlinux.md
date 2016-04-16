#Instalando o Yaourt no Arch Linux

##A instalação do Yaourt é feita adiconando ao seu pacman.conf o repositório "archlinuxfr".

```
#abrindo o pacman.conf
sudo nano /etc/pacman.conf
```

#Coloque o repositório no final :
[archlinuxfr]
SigLevel = Never
Server = http://repo.archlinux.fr/x86_64

Salve e saia.

Por fim instale o pacote:

sudo pacman -Sy yaourt

Depois instale seu pacotes com

sudo yaourt -S nome-do-seu-pacote
