#Autocomplete com sudo usando tecla tab

#Abrir o arquivo .bashrc
sudo nano /home/seu-usuario/.bashrc

#Adicone no final do arquivo
complete -cf sudo

#feche e salve e abra o terminal e teste