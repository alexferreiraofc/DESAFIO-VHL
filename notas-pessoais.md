# Desafio consiste em:
Criar uma aplicação com OpenCms + Postgres + Linux + Nginx com proxy reverso. O objetivo final é acessar a instalação OpenCMS através do NGINX com reverse proxy.

## Devo:
documentar todo o processo, passo a passo, fornecendo comandos utilizados

## Publicar no repo github
Toda a doc criada, incluir qualquer script, **arquivo de config** ou outros artefatos criados ou **MODIFICADOS** como parte do exercicio.

# Tarefas 🚀
- [x] Baixar ISO Ubuntu 22.04
  - ⚠️ Comentário importante sobre a tarefa 1
- [x] Baixar JDK 11
- [x] Baixar Tomcat 9.0.35
- [x] Baixar PostgreSQL 14
- [x] Baixar OpenCms13

- [] Baixar Nginx - Apesar de ser essencial e manter a segurança, pode ser configurado por último via acesso remoto da máquina onde será feito o deploy do projeto.


Após ler a doc, percebi que já existe uma imagem no Hub com o openCMS e as dependencias, porém o banco é MariaDB.


Baixei uma VM do ubuntu 22.04 (jammy)
os seguintes comandos foram rodados:

# Permissão de adm na VM

su -
admin
sudo adduser seu_usuario sudo
su seu_usuario

# atualizando pacotes
sudo apt-get update ; apt-get upgrade -y

# Atualizando JDK 11
sudo apt install openjdk-11-jdk

sudo update-alternatives --config java
escolha a JDK 11

sudo update-alternatives --config javac

- Confira as versões:
java -version
javac -version

# Instalando PostgreSQL 14
sudo apt install postgresql-14

sudo systemctl status postgresql

sudo -u postgres psql

SELECT version();
q

\q


# Tomcat o gato...
https://dlcdn.apache.org


wget https://downloads.apache.org/tomcat/tomcat-9/v9.0.95/bin/apache-tomcat-9.0.95.tar.gz

tar -zxvf apache-tomcat-9.0.5.tar.gz

sudo mv apache-tomcat-9.0.5.tar.gz /opt/tomcat
/opt/tomcat/bin/./startup.sh

wget https://localhost:8080

*cat* no index.html que foi baixado pra confirmar caso não seja feito na OS com GUI
cat index.html | grep successfully

## Retorno esperado:
> "If you're seeing this, you've **successfully** installed Tomcat. Congrats"


# Rodando OpenCms 13
http://www.opencms.org/downloads/opencms/opencms-13.0.zip
unzip opencms-13.0.zip

sudo mv opencms.war /opt/tomcat/webapps

Ele vai começar a extrair uma pasta do 'opencms'

Quando tiver concluido, tentaremos acessar a URL do opencms


localhost:8080/opencms/setup

A partir de agora é necessário fazer uma configuração no postgres para poder "liberar" o OpenCms abrir banco e uma database. [Configurando PostgreSQL](#configurando-postgresql-para-o-setup-do-opencms)


# Configurando Postgresql para o setup do OpenCms

https://www.youtube.com/watch?v=_fdhVbysbnw&t=6s

https://community.qlik.com/t5/Connectivity-Data-Prep/PostgreSQL-Password-forgot/td-p/2160246


Eu tive que alterar o pg_hba.conf para trust para acessar o postgres sem senha
- Alterar a senha do **postgres** para **postgres**

- Diretório:
sudo nano /etc/postgresql/14/main/pg_hba.conf

- Alterar as permissões para trust do ipv4, ipv6, unix, tudo...

sair e trocar para
su -
su - postgres
psql
\password
nova senha = postgres
\q
exit
systemctl restart postgresql


- No OpenCms
Setup Connection:
postgres
postgres
template1

connection
opencms
opencms

DB Name: **opencms**
create db and user [x] Check
Create Tables [x] Check


# NGINX

sudo apt install nginx

sudo nano /etc/nginx/sites-available/default
ou
sudo nano /etc/nginx/conf.d/



server {
    listen 80;
    server_name localhost;

    location / {
        proxy_pass http://127.0.0.1:8080/opencms/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}


/etc/hosts 

Porém o site não está indo, apesar de adicionar o localhost, ele não me direciona ao /opencms, se eu tentar acessar manualmente a URL localhost:8080/opencms eu consigo acessar, agora colocando apenas o 127.0.0.1 ele não entra.


# Tentativa com Docker
- O processo no Docker é muito mais rápido, porém pode haver confusão na comunicação dos serviços e na exposição das portas. Atenção aos volumes

Necessário:
[x]1 - Pasta base dos arquivos
[x]2 - Ubuntu + JDK + Tomcat + OpenCms > Nginx(Instalar por ultimo)
[x]3 - Postgres

O passo 1 é a hierarquia de pastas aqui do vscode, ficando desta forma:
- projeto-docker
  - tomcat/opencms
  - postgres

Como o postgres responde separado apenas expondo a porta 5432, posso deixar ele numa imagem a parte, porém não estou conseguindo fazer o setup do OpenCms, acredito que preciso fazer o passo a passo dentro do postgres pra trocar o password ou liberar trust no pg_hba.conf

Resolvido:
Database Config:
Connection String: jdbc:postgresql://**postgres_container**:5432/
Eu estava deixando o localhost


> teste de trust + senha padrao postgres
ENV POSTGRES_HOST_AUTH_METHOD=trust
RUN POSTGRES_PASSWORD=postgres

>Setup Connection:
postgres
postgres
#template1

>connection config:
opencms
opencms

string url : 
jdbc:postgresql://postgres_container:5432/

Create DB User[x]
Creatle tables [x]

```bash
docker exec -it tomcat_container bash
```
´´´bash
apt-get update
´´´

Executando nginx na imagem do tomcat pra teste
na pasta raiz do projeto onde está o docker-compose.yml execute:

docker exec -it tomcat_container bash
apt get nginx


>> adicionar ao dockerfile 
apt update
apt install net-tools


http://localhost/

retornou alguma coisa no terminal.

Parece que o direcionamento funcionou, estou recebendo resposta das requisições no terminal do projeto.



opencms.local

# DESCONTINUADO ###################################################
## JDK 17
sudo apt install openjdk-17-jdk

java -version

openjdk17 ou 11 - depende do opencms e do tomcat, após o tomcat 10, já utiliza os pacotes Jakarta e não mais java EE
## Tomcat o gato...
https://dlcdn.apache.org


wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.30/bin/apache-tomcat-10.1.30.tar.gz

tar -zxvf apache-tomcat-10.1.30.tar.gz

sudo mv apache-tomcat-10.1.30.tar.gz /opt/tomcat
/opt/tomcat/bin/./startup.sh

wget https://localhost:8080

*cat* no index.html que foi baixado pra confirmar caso não seja feito na OS com GUI
cat index.html | grep successfully

## Retorno esperado:
> "If you're seeing this, you've **successfully** installed Tomcat. Congrats"

## OpenCms
⚠️ Com o tomcat em execução, extrair o arquivo .WAR para dentro do diretório webapps do tomcat, dessa forma ele vai fazer o deploy do opencms.


wget http://www.opencms.org/downloads/opencms/opencms-17.0.zip

unzip opencms-17.0.zip

mv opencms.war /opt/tomcat/webapps

## - > DEPLOY ERRO TOMCAT/OPENCMS: 404 not found
Após algumas tentativas, verificar logs e afins, não consegui compreender o motivo de não fazer o deploy com sucesso do opencms. Optei por remover as versões atuais e tentar com uma versão que use os pacotes java EE ao invés do Jakarta.

Feito alteração dos pacotes
Novos:

OpenCms 13
Tomcat 9.0.35

Obtive sucesso ao atingir o endpoint /opencms/setup

O processo com os arquivos é o mesmo dos anteriores, consiste em instalar o tomcat na pasta /opt/tomcat
e extrair o arquivo do opencms dentro da pasta /opt/tomcat/webapps

Com os links novos é possível utilizar o mesmo passo a passo acima e atingir com sucesso o deploy do opencms
