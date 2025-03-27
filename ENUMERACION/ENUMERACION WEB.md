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


