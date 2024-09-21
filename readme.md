# Docs
https://documentation.opencms.org/opencms-documentation/introduction/get-started/
https://tomcat.apache.org
https://ubuntu.com/download

https://medium.com/@jasonrbodie/learn-linux-install-apache-tomcat-10-and-nginx-on-ubuntu-24-04-5bcdd9fad1c9#b345

# Docker img (mariaDB!)
https://hub.docker.com/r/alkacon/opencms-docker/



# Notas 
- Baixar JDK 17
- Atenção ao [OpenCms](#requisitos) que vai ser instalado, a versão do JDK e tomcat vão mudar se for outra versão de OpenCms
- ⚠️ Tomcat 9 = JDK 11 - Pacotes Java EE
- ⚠️ Tomcat 10 = JDK 17 - Pacotes Jakarta

# Requisitos
Pode ser baixado clicando diretamente aqui ou usando **wget**.
- [Ubuntu](https://releases.ubuntu.com/jammy/ubuntu-22.04.5-desktop-amd64.iso) - 22.04
- Java 17 JDK
  - sudo apt install openjdk-17-jdk
- PostgreSQL 14
  - sudo apt install postgresql-14
- [Tomcat 10.1.30](https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.30/bin/apache-tomcat-10.1.30.tar.gz)
- [OpenCms 17](http://www.opencms.org/downloads/opencms/opencms-17.0.zip)
- Nginx