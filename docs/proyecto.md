# Documentación del Servidor FTP

Este documento describe paso a paso la instalación y configuración de un servidor FTP en Debian 11 utilizando vsftpd.
Incluye evidencias, comandos utilizados, configuración del firewall y pruebas de conexión.

## Entorno de trabajo

- **Sistema operativo:** Debian 11
- **Servicio FTP:** vsftpd
- **Cliente FTP:** FileZilla
- **Usuario FTP creado:** `ftp_user`
- **IP del servidor:** `192.168.1.35 
- **Modo de conexión:** FTP simple (sin cifrado TLS), modo pasivo habilitado

## Plan de despliegue

1. **Instalación y activación de vsftpd**
   ```bash
   sudo apt install -y vsftpd
   sudo systemctl enable --now vsftpd
   sudo systemctl status vsftpd

2. **Configuración del firewall**  
   Edición de /etc/nftables.conf:
   ```bash
   table inet filter {
   	chain input {
   		type filter hook input priority 0;
   		policy drop;

       		ct state established, related accept
       		iif lo accept
       		tcp dport 22 accept  # SSH
   		tcp dport 21 accept  # FTP
  	}

  	chain output {
   		type filter hook output priority 0;
   		policy accept;
  	}
   }

3. **Aplicación de reglas**
   ```bash
   sudo systemctl restart nftables
   sudo nft list ruleset

4. **Configuración segura de vsftpd**
   ```bash
   sudo cp /etc/vsftpd.conf /etc/vsftpd.conf.bak
   sudo nano /etc/vsftpd.con
 
   Opciones clave:
   listen=YES
   listen_ipv6=NO
   anonymous_enable=NO
   local_enable=YES
   write_enable=YES
   chroot_local_user=YES
   allow_writeable_chroot=YES

5. **Creación del usuario FTP y carpeta de pruebas**
   ```bash
   sudo useradd -m ftp_user
   sudo passwd ftp_user
   sudo mkdir /home/ftp_user/ftp_pruebas
   sudo chown ftp_user:ftp_user /home/ftp_user/ftp_pruebas

6. **Habilitación del modo pasivo**
   ```bash
   pasv_enable=YES
   pasv_min_port=30000
   pasv_max_port=31000
   pasv_address=192.168.1.35

7. **Apertura del rango en el firewall**
   ```bash
   sudo nft add rule inet filter input tcp dport 30000-31000 accept

8. **Reinicio del servicio**
   ```bash
   sudo systemctl restart vsftpd
   sudo systemctl status vsftpd
