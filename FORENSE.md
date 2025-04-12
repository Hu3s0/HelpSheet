Parece que estás organizando apuntes sobre análisis forense en Linux y Windows. Te puedo ayudar a estructurarlos mejor y agregar más detalles si lo deseas. Aquí tienes una versión mejorada y más clara:  

---

# 🕵️‍♂️ **Análisis Forense - Gestión de Discos y Archivos**  

## 🔍 **1. Comprobar Unidades de Disco y Particiones**  
Para listar discos y particiones en el sistema:  

- **Linux:**  
  ```bash
  sudo fdisk -l
  ```
  - Muestra información de las particiones del disco.  

  ```bash
  df -h
  ```
  - Muestra el uso del disco con tamaños legibles.  

---

## 📀 **2. Crear Copia de Disco o Partición (Imagen Forense)**  
Clonar una partición o disco en un archivo de imagen:  

- **Usando `dd`**  
  ```bash
  sudo dd if=/dev/sdb1 of=/home/kali/imagenUSB.dd
  ```
  - `if=` → Origen (dispositivo de entrada).  
  - `of=` → Destino (archivo de imagen).  

- **Añadiendo barra de progreso con `pv`**  
  ```bash
  sudo dd if=/dev/sdb1 | pv | sudo dd of=/home/kali/imagenUSB.dd
  ```

- **Usando `dc3dd` (Mejorado con hash y logs)**  
  ```bash
  sudo dc3dd if=/dev/sdb1 of=/home/kali/imagenUSB.dd hash=md5 log=/home/kali/log_hash.txt
  ```
  - Genera un **hash MD5** de la copia y lo guarda en un log.  
  - Se recomienda guardar el hash para verificar la integridad más tarde.  

- **Clonar directamente a otro disco**  
  ```bash
  sudo dd if=/dev/sdb of=/dev/sdc bs=4M status=progress
  ```
  - Copia `sdb` en `sdc` bloque a bloque.  
  - **⚠️ Cuidado**, este comando sobrescribe el disco de destino.  

---

## 🔑 **3. Calcular Hash de una Imagen o Archivo**  
Verificar la integridad de la imagen con hash:  

- **En Linux:**  
  ```bash
  md5sum ImagenUSB.dd
  sha512sum ImagenUSB.dd
  ```

- **En Windows (PowerShell o cmd):**  
  ```powershell
  CertUtil -hashfile ImagenUSB.dd MD5
  CertUtil -hashfile ImagenUSB.dd SHA512
  ```

---

## 🔗 **4. Montar USB o Discos en Linux**  
Para listar discos montados:  
```bash
mount
```

### **Montar manualmente un USB o disco**  
1️⃣ **Crear una carpeta como punto de montaje:**  
```bash
mkdir /home/kali/usb
```
2️⃣ **Montar el dispositivo en la carpeta:**  
```bash
sudo mount /dev/sdb1 /home/kali/usb
```

- **Montar en solo lectura (para análisis forense)**  
  ```bash
  sudo mount -o ro /dev/sdb1 /home/kali/usb
  ```
  - Esto evita modificar accidentalmente el disco.  

### **Desmontar un disco**  
```bash
sudo umount /dev/sdb1
```

---

MEMORIA RAM

EXTRAER
.......

WINDOWS
.......

Con progrma Accesdata FTK imager => Capture memory

LINUX
.....

sudo insmod lime-6.12.20-amd64.ko path=<RUTA> format=raw

ANALIZAR
........

sudo volatility --help > ayudavol.txt => Extrae la ayuda y explica los plugins que podemos usar
    abrimos con: leafpad ayudavol.txt
    
sudo volatility -f ARCHIVO imageinfo => averiguar el perfil 

sudo volatility -f ARCHIVO --profile=WinXPSP2x86 plugin => base de comando + el plugin que queramos usar






