# ✨ Enumeración ✨
---

## 🔍 Descubrimiento de Hosts

### **ARP Scan** (Recomendado ⭐)

> Escanea la red en busca de dispositivos conectados usando ARP.

```bash
arp-scan -I [interfaz] [rango IP] -g
```

### **Fping**

> Envía pings a un rango de IPs y muestra solo las que están activas.

```bash
fping -a -g {Rango de IP}
```

### **Nmap Ping Scan**

> Realiza un escaneo de ping para detectar hosts activos.

```bash
nmap -sn {Rango de IP}
```

### **Netdiscover**

> Escaneo de red en entornos LAN.

```bash
netdiscover -r {Rango de IP}
```

---

## 🔮 Descubrimiento de Puertos

### **Escaneo Rápido de Puertos TCP**

> Escaneo SYN de todos los puertos abiertos.

```bash
sudo nmap -sS --min-rate 5000 -p- --open {IPs} -oN tcp_scan.txt
```

📌 **Parámetros utilizados:**
- `-sS` → Escaneo SYN (no completa la conexión)
- `--min-rate 5000` → Acelera el escaneo
- `-p-` → Escanea todos los puertos
- `--open` → Muestra solo puertos abiertos
- `-oN` → Guarda la salida en un archivo

### **Escaneo Detallado de Puertos Específicos**

> Detecta versiones y usa scripts de reconocimiento.

```bash
nmap -p22,80,443,445,... -sC -sV --open {IPs} -Pn -oN full_scan.txt
```

📌 **Parámetros utilizados:**
- `-sC` → Usa scripts básicos de reconocimiento
- `-sV` → Detecta versiones de servicios
- `-Pn` → Ignora detección de hosts (útil si bloquea ICMP)

---

## 🌐 Enumeración Web

### **Análisis Básico**

> Ver el código fuente de una web.

```bash
Ctrl + U
```

> Modificar el archivo hosts para resolución DNS.

```bash
sudo nano /etc/hosts
```

📌 **Ejemplo:**
```
10.10.10.10 blog.thm
```

### **Análisis de DNS**

```bash
dig axfr [dominio]
```
```bash
nslookup
```

### **Escaneo de Tecnologías Web**

```bash
whatweb -v http://url.com
```

### **Escaneo SSL**

```bash
sslscan <IP>
```
```bash
openssl s_client -connect [dominio]:443
```

### **Descubrimiento de Directorios y Archivos**

```bash
dirb http://IP /path/to/wordlist
```
```bash
dirsearch -u http://[IP]/ -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -t 150 -e php,txt,html -f
```
```bash
gobuster dir -u "ip" -w /usr/share/wordlist/rockyou.txt -t 30 -x .txt,.php,.js
```

---

## 🌐 Escaneo de CMS (WordPress, Joomla, Magento)

### **WordPress**

```bash
wpscan --url http://web.com/carpeta
```

### **Joomla**

```bash
joomscan -u [URL]
```

### **Magento**

```bash
magescan.phar scan:all [URL]
```

---

## 🛡️ Búsqueda de Vulnerabilidades

### **Nikto (Escaneo Web)**

```bash
nikto --url http://web.com
```

```bash
nikto -h scanme.nmap.org
```

```bash
nikto -h https://nmap.org -ssl
```

📌 **Ejemplo de escaneo para vulnerabilidades SQLi:**

```bash
nikto -h $TARGET -T 9 -Format txt -o scans/$TARGET-nikto-80-sqli
```

### **Nmap con Scripts de Servicios**

> Escanear servicios específicos en busca de vulnerabilidades.

```bash
nmap --script=servicio -p"Puerto" <IP>
```

> Ejemplo con FTP:

```bash
nmap --script=ftp -p21 10.10.10.10
```

---

🔗 **Recuerda siempre contar con la autorización correspondiente antes de realizar pruebas de seguridad.**
