# 🕵️ ANONIMATO / TOR

## 🔧 Instalación de TOR

```bash
sudo apt install tor
sudo apt install tor torbrowser-launcher -y
```

## 🚀 Iniciar Servicio TOR (Se inicia cada vez que se reinicia)

```bash
sudo service tor start
sudo service tor status
```

## 🔄 Configurar ProxyChains (Para dirigir el tráfico por Tor)

```bash
sudo nano /etc/proxychains4.conf
```
### Configuración dentro del archivo:
- Borrar `#` en `#dynamic_chain`
- Poner `#` a `strict_chain`
- Al final del archivo, añadir:
  ```
  socks5 127.0.0.1 9050
  ```
- Reiniciar el equipo

📌 **Si vamos a usar Metasploit, añadir la línea:**
```bash
localnet 127.0.0.1 000 255.255.255.255
```

## 🖥 Usar ProxyChains

Se añade `proxychains` delante del comando normal:

```bash
proxychains nmap -vv dominio.com      # Escaneo Nmap anónimo
proxychains dirb <IP> <WORDLIST>      # Fuzzing anónimo
proxychains firefox                   # Navegar usando la red Tor
proxychains nikto -h <IP>             # Escaneo Nikto anónimo
proxychains msfconsole                # Usar Metasploit con ProxyChains
```

## 🔍 Firefox - Anonimato

- Desactivar la opción `media.peerconnection.enabled` en `about:config`

## 🌐 Google Chrome - Anonimato

- Instalar la extensión oficial de Google: **WebRTC Network Limiter**

## 🔒 Extra Anonimato

### Modificar DNS de Google o OpenDNS

```bash
sudo nano /etc/resolv.conf
```
**Opciones recomendadas:**
- [OpenNIC Project](http://servers.opennicproject.org/) → Usar 2 direcciones de distinto Owner
- [Uncensored DNS](https://blog.uncensoreddns.org/) → `91.239.100.100` y `89.233.43.71`

### Usar VPN

- **NordVPN** es una de las mejores opciones

---
🚀 ¡Ahora tu tráfico será más anónimo y seguro!
