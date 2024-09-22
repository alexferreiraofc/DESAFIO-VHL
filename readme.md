# Nota Pessoal
Neste repositório haverá duas formas de atingir o objetivo, elas serão encontradas nos tópicos seguintes.

- Processo manual via VM da ISO do Ubuntu 22.04[#ubuntu](#processo-manual-via-iso-de-uma-vm)
- O processo via Docker será feito [aqui](#docker)

Os problemas e dificuldades encontradas no projeto foram relatadas no arquivo [notas-pessoais](notas-pessoais.md) desse repositório e aqui apenas a documentação oficial.

Foi muito interessante participar desse desafio, espero que a documentação esteja de acordo com o esperado. **Até breve**, Alex.

# Processo Manual via ISO de uma VM
- O processo Manual é feito instalando uma ISO do sistema Ubuntu 22.04 como base, e isso pode ser feito [aqui](#docker-)

## Requisitos
Pode ser baixado clicando diretamente aqui ou usando **wget**.
- [Ubuntu](https://releases.ubuntu.com/jammy/ubuntu-22.04.5-desktop-amd64.iso) - 22.04
- Java 11 JDK
 ```bash
 sudo apt install openjdk-11-jdk
```
- PostgreSQL 14
 ```bash
 sudo apt install postgresql-14
```
- [Tomcat 9.0.95](https://downloads.apache.org/tomcat/tomcat-9/v9.0.95/bin/apache-tomcat-9.0.95.tar.gz)
- [OpenCms 13](http://www.opencms.org/downloads/opencms/opencms-13.0.zip)
- Nginx

## Docs
https://documentation.opencms.org/opencms-documentation/introduction/get-started/
https://tomcat.apache.org
https://ubuntu.com/download

https://medium.com/@jasonrbodie/learn-linux-install-apache-tomcat-10-and-nginx-on-ubuntu-24-04-5bcdd9fad1c9#b345

## Notas
- Baixar JDK 11
- Atenção ao [OpenCms](#requisitos) que vai ser instalado, a versão do JDK e tomcat vão mudar se for outra versão de OpenCms
- ⚠️ Tomcat 9 = JDK 11 - Pacotes Java EE
- ⚠️ Tomcat 10 = JDK 17 - Pacotes Jakarta


## Instalando JDK 11
 ```bash
 sudo apt install openjdk-11-jdk
 java -version
 javac -version
```

## Instalando Tomcat 9.0.35
 ```bash 
wget https://downloads.apache.org/tomcat/tomcat-9/v9.0.95/bin/apache-tomcat-9.0.95.tar.gz
```
 ```bash 
tar -zxvf apache-tomcat-9.0.95.tar.gz
sudo mv apache-tomcat-9.0.95 /opt/tomcat
```

> Testando tomcat
/opt/tomcat/bin/./startup.sh
 ```bash 
wget https://localhost:8080
```

*cat* no index.html que foi baixado pra confirmar caso não seja feito na OS com GUI
cat index.html | grep successfully

## Configurando o PostgreSQL
Alterar o **pg_hba.conf** para **trust** para acessar o postgres sem senha
- [x] Alterar a senha do **postgres** para **postgres**

- **Diretório**:
 ```bash 
sudo nano /etc/postgresql/14/main/pg_hba.conf
```

- Alterar as permissões para trust do ipv4, ipv6, unix, tudo...
![pghba](/images/image-1.png)

CTRL + O = Salvar
CTRL + X = Sair

- Novamente no terminal, execute os comandos na seguinte ordem:

 ```bash 
su -
admin
su - postgres
psql
\password
nova senha = postgres
\q
exit
systemctl restart postgresql
```


## Rodando OpenCms13
Finalmente com tudo configurado, vamos bater na URL do setup do OpenCms
https://localhost:8080/opencms/setup

O setup é simples, as configurações são essas:
No OpenCms
- Setup Connection:
postgres
postgres
template1

- OpenCms Connection:
opencms
opencms

- DB Name: **opencms**
**Create db and user** [x] Check
**Create Tables** [x] Check

NEXT!

Aguardar até o fim da instalação, o site vai abrir automaticamente 🚀🚀 
![Sucesso!](/images/image.png)

## Aplicando NGINX ao projeto
 ```bash
sudo apt install nginx
 
sudo nano /etc/nginx/sites-available/default
```
ou
 ```bash 
sudo nano /etc/nginx/conf.d/
```

 ```nano
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
```

# Docker 🐳

- Antes de executar o projeto, deve ser feito o download do arquivo *opencms.war* [aqui](http://www.opencms.org/downloads/opencms/opencms-13.0.zip)


> **IMPORTANTE⚠️**
Os arquivos devem conter diretórios separados para cada imagem, o arquivo do opencms.war deve estar na pasta projeto/tomcat/opencms a estrutura deve ser assim:
```
|projeto-docker/
|── tomcat (Ubuntu + JDK + Tomcat/opencms/opencms.war > Nginx(Instalar por ultimo))
|    └── opencms/
|── postgres/
|── Nginx
 ```

 O projeto pode ser iniciado acessando a pasta **projeto-docker** e rodando o seguinte comando:
 
 ```bash
 cd projeto-docker
 docker-compose up --build
```
## Acessando o setup do OpenCms

[opencsmsetup](http://localhost:8080/opencms/setup)

![setup](/images/opencms_setup.png)

next > finish

# Nginx...

Estou com uma certa dificuldade em fazer o proxy reverso, mesmo configurando ele permanece na página padrão do nginx, ainda é possível acessar a URL do opencms diretamente

Após algumas horas de tentativa, eu obtive algum progresso ao acessar a URL 

http://localhost/

Ele me direcionou ao site do opencms, porém, ficou todo bugado. Mas obtive retorno no terminal

Adicionar ao arquivo hosts
127.0.0.1 vhl.local

A partir de agora, quando tentar acessar vhl.local no navegador, ele vai direcionar ao site do OpenCms.