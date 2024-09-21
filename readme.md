# Docs
https://documentation.opencms.org/opencms-documentation/introduction/get-started/
https://tomcat.apache.org
https://ubuntu.com/download

https://medium.com/@jasonrbodie/learn-linux-install-apache-tomcat-10-and-nginx-on-ubuntu-24-04-5bcdd9fad1c9#b345

# Docker img (mariaDB!)
https://hub.docker.com/r/alkacon/opencms-docker/

- Baixar JDK 17
- Atenção ao [OpenCms](#requisitos) que vai ser instalado, a versão do JDK e tomcat vão mudar se for outra versão de OpenCms
  ⚠️ Tomcat 9 = JDK 11 - Pacotes Java EE
  ⚠️ Tomcat 10 = JDK 17 - Pacotes Jakarta

# Requisitos
Ubuntu - 22.04
Java 17 JDK
PostgreSQL 14
Tomcat 10.1.30 
OpenCms - 17
Nginx


# Permissão de adm na VM

su -
admin
sudo adduser seu_usuario sudo
su seu_usuario


# atualizando pacotes
sudo apt-get update ; apt-get upgrade -y
