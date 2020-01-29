# :file_folder: Criando Servidor Samba :file_folder:

### Instale o Ubunto Server 18.04.3 LTS  
Sempre instale a versão mais recente e com suporte (LTS), pode ser baixado a ISO no site do Ubuntu [www.ubuntu.com/download](https://ubuntu.com/download/server) , durante a instalação do sistema autorize a intalação do ssh.

![SSH_install.PNG](https://github.com/CaioFranzo/Server_Samba/blob/master/SSH_install.PNG?raw=true)  


### ~$ ifconfig  
Para verificar qual seu IP.

Caso quira fazer acesso remoto local a seu servidor pelo Windows10 basta abrir seu CMD e digitar:  
### C:\Users\Raguel> ssh Nome_Usuario@IP_servidor  

Ou instale o Putty [Download aqui](https://www.ssh.com/ssh/putty/download).  
  
![Putty.PNG](https://github.com/CaioFranzo/Server_Samba/blob/master/Putty.PNG?raw=true)  

# Preparando Samba :computer:

### ~$ sudo apt-get install samba  
Instalando sistema samba.

### ~$ sudo mkdir -p /home/arquivos  
Criando pasta pai, onde tera as pastas de arquivos.  

### ~$ sudo mkdir -p /home/arquivos/publico  
Criando pasta de arquivos publicos (todos teram acesso).  

### ~$ cd /home/arquivos  
Entrando na pastas pai.

### ~$ ls
Ver se as pastas de arquivos foram criadas.

### ~$ sudo chmod -R 777 publico  
Dar pemição a todos, provavel que o nome da pasta ficara grifado, ~$ ls para verficar.

### ~$ sudo chmod -R 770 privado
Restringir permição.  

### ~$ sudo addgroup Nome_Grupo
Criando grupo que recebera usuarios que terão acesso a pasta privada.

### ~$ sudo adduser nome_user  
Criando usuario.

### ~$ sudo adduser nome_user nome_grupo
adicionando usuario ao grupo

### ~$  less /etc/group
Vá para o fim e veja se o seu grupo e usuarios foram criados.  

### ~$ sudo chown root.nome_grupo privado/
Adicionando o grupo como proprietario da pasta.

### ~$ sudo chmod g+s privado/
Herdar grupo como permição. farzer para cada pasta restrita do servidor.

### ~$ ll
As pastas devem estar dessa forma.

![Folders.PNG](https://github.com/CaioFranzo/Server_Samba/blob/master/Folders.PNG?raw=true)  

# Configurando arquivos :bookmark_tabs:

### ~$ sudo nano /etc/samba/smb.conf
Arquivo de configuração de pastas do samba.  

### Insira oseguinte codigo ao fim do arquivo:  

[privado]  
comment = Pasta de arquivos com autenticacao  
path = /home/arquivos/privado  
writable = yes  
browseable = yes  
create mode = 0770  
directory mode = 0770  
guest ok = no  
read only = no  
valid users = @merenda *Usar @ para grupos  

[publico]  
comment = Pasta de arquivos publicos  
path = /home/arquivos/publico  
writable = yes  
browseable = yes  
create mode = 0777  
directory mode = 0777  
guest ok = no  
read only = no  

# Configurando ip fixo :space_invader:

A nova versão do Ubuntu 18.04 mudou sua forma de IP estático. Para fazer essa habilitação siga os paços:  
   
### ~$ cd /etc/netplan/
pasta do arquivo que comanda a placa de rede.

### ~$ sudo nano 50-cloud-init.yaml
Abrir o arquivo com o editor NANO ou outro de sua preferencia.  

### O arquivo deve ter as seguintes configurações:  

![Netplan.PNG](https://github.com/CaioFranzo/Server_Samba/blob/master/Netplan.PNG?raw=true)  

### ~$ sudo netplan apply
Testara as alterações.  

Após isso ser servido estara pronto, agora basta em uma maquina Windows enrar no servidor prescionando Win+r e digitando no exucutar \\ip_servidor.  

### ~$ sudo reboot
### E pronto seu servidor de arquivos estara pronto para uso. Espero ter ajudado :smile:
