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

1. **Instalación del servicio vsftpd**
   ```bash
   sudo apt update
   sudo apt install vsftpd

2. **Configuración básica del servicio**
   ```bash
   sudo systemctl enable vsftpd
   sudo systemctl start vsftpd

3. **Creación del usuario FTP**
   ```bash
   sudo adduser ftp_user

4. **Configuración del firewall con nftables**
   ```bash
   sudo nft add rule inet filter input tcp dport 21 accept


5. **Habilitación del modo pasivo**
   ```bash
   pasv_enable=YES
   pasv_min_port=30000
   pasv_max_port=31000
   pasv_address=192.168.1.35

   sudo nft add rule inet filter input tcp dport 30000-31000 accept


6. **Reinicio del servicio**
   ```bash
   sudo systemctl restart vsftpd
