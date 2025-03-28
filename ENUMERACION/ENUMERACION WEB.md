# 🔍 **Tips de Enumeración Web** 🚀  

## 🏗 **Recolecta Información Inicial**  
Antes de lanzar escaneos activos, investiga pasivamente:  
✅ **Revisar el código fuente** (Ctrl + U en el navegador).  
✅ **Buscar comentarios o endpoints ocultos** en el HTML y JavaScript.  
✅ **Identificar tecnologías con Wappalyzer o WhatWeb**  
✅ **Consultar registros en servicios como Shodan o Censys**  
✅ **Verificar archivos sensibles expuestos (`robots.txt`, `.git/`, `.env`, etc.)**  

## 🌐 **Si la url no carga**
hay que añadir la referencia DNS en /etc/hosts/ hay varias formas:
```bash
sudo nano /etc/hosts => añadir referencia DNS ej: 10.10.10.10    blog.thm
```
```bash
sudo sh -c 'echo "10.10.10.10  academy.htb" >> /etc/hosts'
```
```bash
echo "10.10.10.10  academy.htb" | sudo tee -a /etc/hosts
```
---

## 🌐 **Enumeración DNS**  

La **enumeración DNS** es un paso clave en el reconocimiento de una infraestructura web. Permite descubrir subdominios, registros DNS y configuraciones erróneas que pueden ser explotadas.  

### 🔍 **Recolectar Información del Dominio**  
Antes de lanzar ataques activos, podemos obtener información básica sobre el dominio objetivo.  

🔹 **Consultar registros DNS con `nslookup`**  
```bash
nslookup example.com
```
📌 **Para ver los servidores de nombres (NS)**  
```bash
nslookup -type=NS example.com
```
📌 **Para obtener el registro MX (Correo electrónico)**  
```bash
nslookup -type=MX example.com
```

🔹 **Consultar información con `dig`**  
```bash
dig example.com
```
📌 **Para ver los servidores DNS del dominio**  
```bash
dig NS example.com
```
📌 **Para obtener direcciones IP asociadas (A y AAAA records)**  
```bash
dig A example.com
dig AAAA example.com
```

---

### 🌍 **Transferencia de Zona (AXFR)**
Si el servidor DNS está mal configurado, se puede extraer información sensible con **`AXFR`**.  

🔹 **Usar `dig` para probar una transferencia de zona**  
```bash
dig axfr @ns1.example.com example.com
```
🔹 **Otra forma con `host`**  
```bash
host -l example.com ns1.example.com
```
📌 **Si esto funciona, es una mala configuración y puede exponer subdominios, registros internos y más.**  

---

# 🔍 **Herramientas de Enumeración Web** 🚀 

## 🔍 **WhatWeb**  

**WhatWeb** es una herramienta utilizada para **identificar tecnologías web** en un sitio. Es útil en la fase de **reconocimiento** en pentesting, ya que permite obtener información clave sobre el objetivo sin necesidad de realizar ataques activos.  

---

### 🛠 **¿Qué información puede detectar?**  
✅ **CMS usado** (WordPress, Joomla, Drupal, etc.).  
✅ **Tipo y versión del servidor web** (Apache, Nginx, IIS).  
✅ **Lenguajes y frameworks** (PHP, ASP.NET, Django, Laravel).  
✅ **Bibliotecas JavaScript** (jQuery, React, Angular).  
✅ **Plugins y herramientas de analítica** (Google Analytics, Facebook Pixel).  
✅ **Cabeceras HTTP y metadatos** que pueden revelar configuraciones débiles.  

---

### 🚀 **Ejemplo de Uso**  

Para analizar un sitio web:  
```bash
whatweb example.com
```
📌 **Salida típica:**  
```
http://example.com [200] WordPress, Apache[2.4.41], PHP[7.4.3], Google-Analytics
```
Aquí podemos ver que el sitio usa **WordPress, Apache 2.4.41 y PHP 7.4.3**, lo que puede ayudar a buscar vulnerabilidades específicas.  

📌 **Ejemplo de análisis avanzado:**  
```bash
whatweb -a 3 -v example.com
```
Este comando realiza un escaneo **más agresivo y detallado**. 

---

## 🔒 **SSLScan**  

**SSLScan** es una herramienta que permite **analizar la configuración SSL/TLS** de un servidor web. Es útil en **pentesting** y auditorías de seguridad para detectar **cifrados débiles, protocolos obsoletos y vulnerabilidades** en la comunicación segura de un sitio.  

---

### 🛠 **¿Qué información obtiene SSLScan?**  
✅ **Versiones de TLS y SSL soportadas** (TLS 1.3, TLS 1.2, SSLv3, etc.).  
✅ **Cifrados admitidos** (AES, DES, RC4, etc.).  
✅ **Vulnerabilidades conocidas** (POODLE, BEAST, Heartbleed, etc.).  
✅ **Soporte para Perfect Forward Secrecy (PFS)**.  
✅ **Estado del certificado SSL** (emisor, fechas de expiración, etc.).  

---

### 🚀 **Ejemplo de Uso**  

🔹 **Escanear un dominio para ver su configuración SSL/TLS**  
```bash
sslscan example.com
```
```bash
sslscan <IP>
```
📌 **Salida típica:**  
```
Supported Server Cipher(s):
  TLSv1.2  256 bits  ECDHE-RSA-AES256-GCM-SHA384
  TLSv1.2  128 bits  ECDHE-RSA-AES128-GCM-SHA256
```
Esto indica qué cifrados admite el servidor y su nivel de seguridad.  

---

## 🔐 **OpenSSL**  

**OpenSSL** es una **biblioteca de software** y una **herramienta de línea de comandos** utilizada para trabajar con **protocolos criptográficos** como SSL/TLS. Se usa ampliamente para la **gestión de certificados**, **cifrado de datos** y **verificación de la seguridad** en sistemas y aplicaciones que requieren comunicación segura.

---

### **Verificación de Certificados y Conexiones SSL/TLS**
Con OpenSSL puedes verificar la validez de los certificados SSL/TLS y la configuración de las conexiones seguras en servidores web.

🔹 **Conectar a un servidor y obtener detalles SSL/TLS:**  
```bash
openssl s_client -connect example.com:443
```
Este comando abre una conexión SSL/TLS a `example.com` en el puerto 443 y muestra detalles sobre el certificado y la sesión.

---

## 💻 **WFuzz**  

**WFuzz** es una herramienta de **fuzzing** de aplicaciones web, diseñada específicamente para realizar pruebas de **penetración** al descubrir vulnerabilidades de seguridad en sitios web. Permite realizar ataques de **fuerza bruta** y **fuzzing** sobre parámetros de URL, formularios web, encabezados HTTP y otros puntos de entrada para encontrar fallos de seguridad, como **inyección de comandos**, **cross-site scripting (XSS)**, **autenticación débil**, **vulnerabilidades de directorios**, entre otros.

---

### **Comando:**
```bash
wfuzz -c -t 400 --hc=404 -w [wordlist] [url]/FUZZ
```
**`-c`**: Esta opción habilita el **modo colorido**, lo que facilita la visualización de los resultados con diferentes colores.

**`-t 400`**: Este parámetro define el número de **hilos concurrentes** que WFuzz utilizará al realizar el fuzzing. En este caso, `-t 400` significa que se enviarán hasta 400 solicitudes simultáneas.

**`--hc=404`**: Esto le indica a WFuzz que **ignore las respuestas con el código de estado HTTP 404**

**`-w [wordlist]`**: Especifica la **lista de palabras** que se utilizarán para realizar el fuzzing. Por ejemplo, `rockyou.txt` o `dirbuster.txt`)

**`[url]/FUZZ`**: Es la URL objetivo donde se realizará el fuzzing. La palabra clave `FUZZ` es donde WFuzz sustituirá cada entrada de la lista de palabras para probar diferentes rutas en el servidor. 
   ```
   http://example.com/admin  
   http://example.com/login  
   http://example.com/dashboard  
   ```
   Y así sucesivamente.

---

### **Ejemplo completo**:

```bash
wfuzz -c -t 400 --hc=404 -w /opt/wordlists/directory-list-2.3-small.txt http://example.com/FUZZ
```

---

### **Comando:**
```bash
wfuzz -c -t 400 --hc=404 -w [wordlist] -w [archivo_extensiones] [url]/FUZZ.FUZ2Z
```
---

### **Explicación de los parámetros:**

**`[url]/FUZZ.FUZ2Z`**: La palabra clave `FUZZ` será reemplazada por cada palabra de la lista de directorios/archivos, y `FUZ2Z` será reemplazada por las extensiones de archivo que aparecen en la lista de extensiones. 

   Si el contenido de `[wordlist]` es:
   ```
   admin
   login
   dashboard
   ```
   Y el contenido de `[archivo_extensiones]` es:
   ```
   .php
   .html
   .asp
   ```
   WFuzz probará las siguientes combinaciones:
   ```
   http://example.com/admin.php
   http://example.com/admin.html
   http://example.com/admin.asp
   http://example.com/login.php
   http://example.com/login.html
   http://example.com/login.asp
   http://example.com/dashboard.php
   http://example.com/dashboard.html
   http://example.com/dashboard.asp
   ```

---

### **Ejemplo Completo**:

```bash
wfuzz -c -t 400 --hc=404 -w /opt/wordlists/directory-list-2.3-small.txt -w /opt/wordlists/extensions.txt http://example.com/FUZZ.FUZ2Z
```

---

#### **Fuzzing de Parámetros de Formularios Web**  
🔹 **Formulario con parámetros GET**  
```bash
wfuzz -c -w /ruta/a/lista_de_palabras.txt -u "http://example.com/page.php?id=FUZZ"
```
Esto prueba los valores de la lista de palabras como el parámetro `id`.

🔹 **Formulario con parámetros POST**  
```bash
wfuzz -c -w /ruta/a/lista_de_palabras.txt -X POST -d "username=FUZZ&password=test" http://example.com/login
```
#### **Fuzzing de Encabezados HTTP**  

```bash
wfuzz -c -w /ruta/a/lista_de_palabras.txt -H "User-Agent: FUZZ" http://example.com
```

#### **Fuzzing de Variables de Cookies y Autenticación**  

🔹 **Fuzzing de cookies**  
```bash
wfuzz -c -w /ruta/a/lista_de_palabras.txt -H "Cookie: sessionid=FUZZ" http://example.com
```
## FFUF

**FFUF (Fast Web Fuzzer)** es una herramienta rápida y flexible para la enumeración de directorios, archivos, subdominios y parámetros en aplicaciones web. Permite automatizar la búsqueda de rutas y recursos ocultos mediante diccionarios.

---
### 📌 Enumeración de Directorios
Escanea directorios en una aplicación web usando un diccionario de rutas:
```bash
ffuf -w /opt/useful/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://83.136.252.13:39937/FUZZ
```

---
### 📌 Enumeración de Extensiones
Encuentra archivos con extensiones específicas en una URL:
```bash
ffuf -w /opt/useful/seclists/Discovery/Web-Content/web-extensions.txt:FUZZ -u http://SERVER_IP:PORT/blog/indexFUZZ
```

---
### 📌 Enumeración de Páginas
Busca páginas web ocultas con diferentes nombres:
```bash
ffuf -w /opt/useful/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/blog/FUZZ.php
```

---
### 📌 Enumeración Recursiva
Escanea directorios de manera recursiva y busca archivos con extensiones específicas:
```bash
ffuf -w /opt/useful/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ -recursion -recursion-depth 1 -e .php -v
```

---
### 📌 Enumeración de Subdominios
Encuentra subdominios activos dentro de un dominio objetivo:
```bash
ffuf -w /opt/useful/seclists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://FUZZ.academy.htb/
```

---
### 📌 Filtrar Resultados por Tamaño
Filtra los resultados en función del tamaño de respuesta:
```bash
ffuf -w /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://academy.htb:36574/ -H 'Host: FUZZ.academy.htb' -fs 986
```

---
### 📌 Enumeración de Parámetros
Descubre parámetros ocultos en una URL:
```bash
ffuf -w /opt/useful/seclists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://admin.academy.htb:50526/admin/admin.php?FUZZ=key -fs 798
```
Para métodos POST:
```bash
ffuf -w /opt/useful/seclists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'FUZZ=key' -H 'Content-Type: application/x-www-form-urlencoded' -fs xxx
```

---
### 📌 Enumeración de Valores
Encuentra valores válidos en parámetros:
```bash
ffuf -w /usr/share/seclists/Usernames/xato-net-10-million-usernames.txt:FUZZ -u http://vhost.academy.htb:PORT/directory/page.ext -X POST -d "parameter=FUZZ" -H "Content-Type: application/x-www-form-urlencoded" -fs xxx
```
Generar un diccionario de IDs del 1 al 1000:
```bash
for i in $(seq 1 1000); do echo $i >> ids.txt; done
```
Usarlo en una petición:
```bash
ffuf -w ids.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'id=FUZZ' -H 'Content-Type: application/x-www-form-urlencoded' -fs xxx
```

---
📌 Otras Enumeraciones con FFUF

🔍 Búsqueda de LFI (Local File Inclusion):
```bash
ffuf -w /usr/share/wordlists/seclists/Fuzzing/LFI/LFI-Jhaddix.txt -u http://<RHOST>/admin../admin_staging/index.php?page=FUZZ -fs 15349
```
🔍 Enumeración de nombres de usuario mediante respuesta del servidor:
```bash
ffuf -w /usr/share/wordlists/SecLists/Usernames/Names/names.txt -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -u http://MACHINE_IP/customers/signup -mr "username already exists"
```
🔍 Escaneo de subdominios:
```bash
ffuf -c -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-20000.txt -H "host:FUZZ.empline.thm" -u 'http://empline.thm/' -fs 14058
```

## **DIRB**  

**DIRB** es una herramienta utilizada para la fuerza bruta de directorios y archivos en aplicaciones web. Funciona probando cada palabra de un diccionario en la URL objetivo para descubrir contenido oculto, como páginas no indexadas o configuraciones sensibles.  

---

### **Explicación del comando:**  
```bash
dirb http://IP /path/to/wordlist
```
📌 **Componentes del comando:**  
1. **`dirb`** → Ejecuta la herramienta.  
2. **`http://IP`** → URL o IP del servidor web que se va a escanear.  
3. **`/path/to/wordlist`** → Ruta del diccionario de palabras a usar en la enumeración.  

---

### **Ejemplo de uso práctico:**  
Si queremos escanear el servidor `192.168.1.100` usando el diccionario predeterminado de DIRB, ejecutamos:  
```bash
dirb http://192.168.1.100
```
📌 **¿Qué hace esto?**  
- Prueba diferentes rutas comunes en la web (`admin/`, `login/`, `uploads/`, etc.).  
- Usa el diccionario por defecto de DIRB (`/usr/share/dirb/wordlists/common.txt`).  

---

### **Opciones útiles en DIRB:**  
✅ **Ignorar códigos de error específicos:**  
```bash
dirb http://192.168.1.100 -X .php,.html
```
→ Solo buscará archivos con extensiones `.php` y `.html`.  

✅ **Usar un agente de usuario personalizado (evitar bloqueos):**  
```bash
dirb http://192.168.1.100 -a "Mozilla/5.0 (Windows NT 10.0; Win64; x64)"
```
→ Simula una petición desde un navegador común.  

✅ **Guardar los resultados en un archivo:**  
```bash
dirb http://192.168.1.100 -o resultados.txt
```
→ Exporta la información encontrada a `resultados.txt`.  

---
### **DIRB con Autenticación Básica (HTTP Basic Auth)**  

Si al acceder a una URL aparece un cuadro emergente solicitando credenciales (Autenticación Básica HTTP), podemos usar DIRB para escanear las rutas protegidas utilizando las credenciales correctas.

---

### **Explicación del comando:**  
```bash
dirb http://[IP]/ -u user:password
```
📌 **Componentes:**  
1. **`dirb`** → Ejecuta la herramienta.  
2. **`http://[IP]/`** → Dirección del servidor web que queremos escanear.  
3. **`-u user:password`** → Envía las credenciales para autenticación básica.

---

### **Ejemplo de uso:**  
Si intentamos acceder a `http://192.168.1.100/admin` y nos pide un usuario y contraseña, podemos usar:  
```bash
dirb http://192.168.1.100/admin -u admin:admin123
```
✅ Esto permite realizar la fuerza bruta de directorios dentro de la sección protegida del sitio web.

---

## **DIRSEARCH**  

**Dirsearch** es una herramienta de enumeración de directorios y archivos en servidores web. Es más rápida y flexible que DIRB, permitiendo el uso de múltiples hilos (**threads**) y escaneo de extensiones específicas.  

---

### **Explicación del comando:**  
```bash
dirsearch -u http://[IP]/ -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -t 150 -e php,txt,html -f
```

📌 **Componentes:**  
1. **`-u http://[IP]/`** → Define la URL objetivo a escanear.  
2. **`-w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt`** → Usa un diccionario para buscar rutas ocultas.  
3. **`-t 150`** → Usa 150 hilos (**threads**) para hacer el escaneo más rápido.  
4. **`-e php,txt,html`** → Busca archivos con las extensiones `.php`, `.txt`, `.html`.  
5. **`-f`** → Fuerza el escaneo de todas las extensiones, incluso si la URL base no responde.

---

### **Ejemplo de uso práctico:**  
Si queremos escanear un servidor en `http://192.168.1.100/` buscando archivos `.php`, `.txt` y `.html`, usamos:  
```bash
dirsearch -u http://192.168.1.100/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 100 -e php,txt,html -f
```
🔹 **¿Qué hace este comando?**  
- Prueba rutas desde el diccionario para encontrar directorios y archivos ocultos.  
- Usa **100 hilos** para acelerar la búsqueda.  
- Filtra los resultados por archivos de tipo `.php`, `.txt` y `.html`.  
- **No se detiene ante errores 403 o 404**, sigue forzando el escaneo.

---

### **Opciones útiles en Dirsearch:**  
✅ **Autenticación básica HTTP:**  
```bash
dirsearch -u http://192.168.1.100/ -w wordlist.txt -t 50 -e php -f -A "admin:password"
```
→ Si el sitio web tiene autenticación básica, pasamos usuario y contraseña.  

✅ **Personalizar cabeceras (cookies, user-agents, etc.):**  
```bash
dirsearch -u http://192.168.1.100/ -w wordlist.txt -H "Cookie: session=abc123"
```
→ Útil si el sitio usa sesiones autenticadas con cookies.  

✅ **Guardar los resultados en un archivo:**  
```bash
dirsearch -u http://192.168.1.100/ -w wordlist.txt -t 100 -e php -f -o resultado.txt
```
→ Exporta los directorios encontrados a `resultado.txt`.  

---
## **DIRBUSTER**  

**DirBuster** es una herramienta de enumeración de directorios y archivos en servidores web con una interfaz gráfica (**GUI**). Es una alternativa a herramientas como **DIRB, FFUF y Dirsearch**, permitiendo configuraciones más visuales y personalizadas.  

---

### **Pasos para usar DirBuster:**  
1️⃣ **Abrir DirBuster:**  
   - En Kali Linux, ejecútalo con:  
     ```bash
     dirbuster
     ```
  
2️⃣ **Configurar el **Target** (Objetivo):**  
   - En la casilla **"Target URL"**, introduce la dirección:  
     ```
     http://10.10.10.10
     ```

3️⃣ **Seleccionar el Diccionario:**  
   - Clic en **"Browse"** para elegir un diccionario.  
   - Usa el archivo recomendado:  
     ```
     /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
     ```

4️⃣ **Configurar Opciones Adicionales (Opcional):**  
   - **Número de hilos (Threads):** Ajustar a **50-100** para mayor velocidad.  
   - **Extensiones a buscar:** `.php`, `.html`, `.txt`, `.bak`, `.log`, etc.  
   - **Depth (Profundidad de escaneo):** Configurar según necesidad.  

5️⃣ **Iniciar el Escaneo:**  
   - Presiona **"Start"** y observa los resultados en tiempo real.  

---

## **Gobuster**  

**Gobuster** es una herramienta rápida y eficiente para realizar fuerza bruta sobre directorios, archivos y subdominios en servidores web. Se usa en pentesting para descubrir rutas ocultas y estructuras de sitios web.  

---

## 📌 **Enumeración de Directorios con Gobuster**  

### **Ejemplo básico:**  
```bash
gobuster dir -u "http://IP" -w /usr/share/wordlists/rockyou.txt -t 30
```
📌 **Explicación de los parámetros:**  
- **`dir`** → Modo de escaneo de directorios.  
- **`-u "http://IP"`** → URL objetivo.  
- **`-w /usr/share/wordlists/rockyou.txt`** → Ruta del diccionario.  
- **`-t 30`** → Número de hilos (**threads**) usados en el escaneo.  
  - Más hilos = escaneo más rápido pero más ruido en la red.  

---

### **Escaneo con Extensiones Específicas**  
Si queremos buscar archivos con extensiones como `.txt`, `.php` o `.js`, agregamos `-x`:  
```bash
gobuster dir -u "http://IP" -w /usr/share/wordlists/rockyou.txt -t 30 -x .txt,.php,.js
```
📌 **Explicación adicional:**  
- **`-x .txt,.php,.js`** → Filtra archivos con estas extensiones.  
- Esto ayuda a encontrar archivos sensibles como `config.php`, `backup.txt`, etc.

---

## 📌 **Enumeración de Subdominios con Gobuster**  

Gobuster también permite buscar **subdominios activos** en un dominio objetivo.  

```bash
gobuster dns -d inlanefreight.com -w /usr/share/SecLists/Discovery/DNS/namelist.txt
```
📌 **Explicación de los parámetros:**  
- **`dns`** → Modo de escaneo de subdominios.  
- **`-d inlanefreight.com`** → Dominio objetivo.  
- **`-w /usr/share/SecLists/Discovery/DNS/namelist.txt`** → Diccionario de nombres de subdominios.  

🔹 **Ejemplo de resultado:**  
```
admin.inlanefreight.com
mail.inlanefreight.com
dev.inlanefreight.com
```
➡️ Estos subdominios pueden contener aplicaciones ocultas, paneles de administración o servicios sensibles.

---

## **Escaneo de CMS con WPScan, JoomScan, Droopescan y MageScan**  

Las herramientas de escaneo de CMS (**Content Management System**) permiten descubrir vulnerabilidades en plataformas como **WordPress, Joomla, Drupal y Magento**.  

---

## **📌 Escaneo de WordPress con WPScan**  

### **Escaneo básico de un sitio WordPress**  
```bash
wpscan --url http://web.com/carpeta
```
📌 **Explicación de los parámetros:**  
- **`--url http://web.com/carpeta`** → URL del sitio objetivo.  
- Escanea temas, plugins y versiones de WordPress.  

### **Escaneo con detección de usuarios**  
```bash
wpscan --url http://web.com -e u
```
📌 **`-e u`** → Enumera usuarios registrados, útil para ataques de fuerza bruta.  

### **Escaneo con API para más detalles**  
```bash
wpscan --url http://web.com --api-token TU_API_TOKEN
```
📌 **WPScan tiene una base de datos de vulnerabilidades** en WordPress, y con un token gratuito o de pago, se obtienen más detalles.  

---

## **📌 Escaneo de Joomla con JoomScan**  

```bash
joomscan -u http://web.com
```
📌 **Explicación de los parámetros:**  
- **`-u http://web.com`** → URL objetivo.  
- Detecta la versión de Joomla, vulnerabilidades, extensiones y configuraciones inseguras.  

---

## **📌 Escaneo de Drupal con Droopescan**  

Droopescan permite analizar sitios creados con **Drupal, SilverStripe y otras plataformas CMS**.  

### **Instalación de Droopescan**  
```bash
git clone https://github.com/SamJoan/droopescan.git
cd droopescan
pip3 install -r requirements.txt
```

### **Escaneo de Drupal**  
```bash
droopescan scan drupal -u http://web.com
```
📌 **Explicación de los parámetros:**  
- **`scan drupal`** → Indica que es un sitio Drupal.  
- **`-u http://web.com`** → URL objetivo.  

---

## **📌 Escaneo de Magento con MageScan**  

MageScan se usa para detectar vulnerabilidades en sitios **Magento**.  

### **Instalación de MageScan**  
```bash
git clone https://github.com/steverobbins/magescan.git
cd magescan
composer install
```

### **Ejecutar un escaneo completo**  
```bash
php magescan.phar scan:all http://web.com
```
📌 **Explicación de los parámetros:**  
- **`scan:all`** → Realiza un escaneo completo del sitio Magento.  
- **`http://web.com`** → URL objetivo.  

---

## **ZAProxy**

**OWASP ZAP (Zed Attack Proxy)** es una herramienta de seguridad popular para realizar pruebas de penetración y auditorías de seguridad en aplicaciones web. ZAP proporciona una serie de características que facilitan la **enumeración** de directorios, parámetros, sesiones, vulnerabilidades y más.

---

Abre ZAP con:

```bash
zaproxy
```

---

### **Proxies y Configuración de Navegador**

ZAP actúa como un **proxy intermediario** entre tu navegador y la aplicación web que deseas analizar. Para configurarlo, sigue estos pasos:

1. **Configura el proxy en tu navegador:**
   - En el navegador, configura la dirección del proxy como `localhost` y el puerto como `8080` (por defecto).
   - Esta configuración permite que el tráfico del navegador pase a través de ZAP.

2. **Inicia ZAP:**
   - ZAP comenzará a interceptar el tráfico HTTP/HTTPS y lo mostrará en su interfaz.

---

### **Enumeración de Directorios con ZAP**

Para enumerar directorios y archivos, ZAP tiene la funcionalidad **Active Scan** y puedes usar el **Spider** para rastrear el sitio web:

#### **Uso del Spider (Rastreo de Páginas)**

- **Acción:**
  1. Navega a **"Spider"** en ZAP.
  2. Especifica la URL del sitio web y haz clic en "Start Scan".
  
- ZAP recorrerá el sitio web, buscando directorios, parámetros y archivos.

#### **Configuración de Active Scan (Escaneo Activo)**

- **Acción:**
  1. Dirígete a **"Target"** en la interfaz de ZAP.
  2. Haz clic derecho en el sitio objetivo y selecciona **"Attack"** → **"Active Scan"**.
  3. ZAP realizará un escaneo activo sobre el sitio, buscando vulnerabilidades comunes, archivos sensibles y directorios ocultos.

---

### **Enumeración de Parámetros y Válidos con ZAP**

ZAP también permite enumerar parámetros en formularios web, como los parámetros GET o POST. Para esto, puedes:

1. **Interceptar el tráfico** con ZAP cuando interactúas con un formulario.
2. **Usar el "Request Editor"** para modificar solicitudes y probar diferentes entradas en los parámetros.
3. **Realizar un escaneo activo** sobre las páginas que contienen formularios o parámetros de entrada.

---

### **Enumeración de Subdominios y Servicios con ZAP**

ZAP también puede ayudar a encontrar subdominios activos usando su herramienta **Passive Scan**. Para ello:

1. **Activa un "Passive Scan"** en la configuración.
2. **Analiza las cabeceras HTTP** y respuestas para encontrar subdominios de los servicios asociados con el dominio.

Puedes usar también herramientas adicionales como **"DNS Interception"** o cargar un archivo de diccionario para buscar subdominios con **ZAP**.

---

### **Informes y Resultados**

ZAP ofrece la opción de generar informes detallados sobre las vulnerabilidades encontradas, lo que incluye:

- **Vulnerabilidades de seguridad** detectadas durante el escaneo activo.  
- **Parámetros** y rutas descubiertas a través de la Spider.  
- **Cualquier posible acceso no autorizado** a partes de la web.

### **Generación de Informe:**
- Dirígete a **"Report"** y selecciona **"Generate Report"** para crear un informe HTML o XML con los resultados del escaneo.

---

### **Burp Suite**

**Burp Suite** es una de las herramientas más poderosas para realizar pruebas de seguridad en aplicaciones web. Permite interceptar y modificar el tráfico HTTP, realizar escaneos de vulnerabilidades y realizar enumeración de directorios, parámetros y más. Aquí te dejo una pequeña guía de enumeración utilizando Burp Suite.

---

### **Configuración del Proxy de Burp Suite**

Burp Suite actúa como un **proxy** entre tu navegador y el servidor web. Debes configurarlo para que todo el tráfico web pase por él.

1. **Abrir Burp Suite:**
   - Ve a la pestaña **Proxy** → **Intercept**.
   - Asegúrate de que la opción **Intercept is on** esté activada.
   
2. **Configurar tu navegador:**
   - Configura el proxy de tu navegador para apuntar a **localhost** y **puerto 8080** (por defecto en Burp).
   - Para Firefox: Configura el proxy en **Preferencias** → **Configuración de red**.

---

### **Interceptar y Modificar Tráfico**

Con el proxy configurado, Burp capturará todo el tráfico HTTP/HTTPS entre tu navegador y el servidor web.

- **Ver solicitudes HTTP**: En la pestaña **Intercept**, podrás ver todas las solicitudes HTTP que realiza tu navegador al servidor.
- **Modificar solicitudes**: Puedes modificar las solicitudes directamente desde Burp antes de que lleguen al servidor (por ejemplo, cambiar parámetros o cabeceras).

---

### **Enumeración de Directorios y Archivos con Burp Suite**

Burp Suite incluye varias herramientas para la enumeración de directorios y archivos, como **Spider** y **Intruder**.

#### **Spider**: Rastrear el Sitio Web

1. Ve a la pestaña **Target** → **Site map**.
2. Haz clic derecho sobre el sitio objetivo y selecciona **"Spider this host"**.
3. Burp comenzará a rastrear todas las páginas del sitio, descubriendo directorios y enlaces ocultos.

#### **Intruder**: Fuzzing y Enumeración de Directorios

**Intruder** permite realizar ataques de fuerza bruta y enumeración utilizando diccionarios. Se puede usar para buscar rutas, parámetros y más.

1. **Configura el Intruder**:
   - Ve a la pestaña **Intruder** → **Positions**.
   - Selecciona la solicitud que deseas fuzzear (por ejemplo, una URL con parámetros o una ruta).
   - Marca los parámetros que deseas enumerar, como directorios o valores.

2. **Configura el diccionario**:
   - En **Payloads**, selecciona el diccionario que quieres usar para la enumeración (puedes usar diccionarios predefinidos o crear el tuyo propio).
   
3. **Inicia el ataque**:
   - Haz clic en **Start attack**. Burp intentará hacer solicitudes con todos los valores del diccionario y capturar las respuestas del servidor.

---

### **Enumeración de Parámetros con Burp Suite (Intruder)**

Puedes usar **Intruder** para enumerar parámetros específicos, como nombres de parámetros ocultos, identificadores de usuarios, etc.

1. **Configurar la solicitud**: 
   - Captura una solicitud que tenga parámetros (GET o POST).
   - Ve a la pestaña **Intruder** → **Positions** y selecciona el parámetro que deseas fuzzear.

2. **Añadir un diccionario**: 
   - En **Payloads**, selecciona un diccionario de parámetros (por ejemplo, nombres de parámetros comunes o valores para probar).

3. **Ejecutar el ataque**:
   - Haz clic en **Start attack** y Burp probará todas las combinaciones de valores en ese parámetro.

---

### **Filtros y Análisis de Resultados**

- En Burp Suite, puedes filtrar resultados para identificar respuestas interesantes, como aquellos con **código de estado HTTP 200**, que indican páginas válidas.
- Revisa las respuestas en la pestaña **Intruder** para encontrar patrones o posibles vulnerabilidades.

---

### **Usar Burp Suite para Enumerar Subdominios**

Burp Suite también permite realizar un análisis pasivo de subdominios y otros recursos, especialmente cuando se usa junto con herramientas externas o complementos, como **Burp Extensions**.

Puedes agregar **Burp Extensions** desde **"Extender"** → **"BApp Store"**, y buscar extensiones para **enumeración de DNS** y **subdominios**.

---
