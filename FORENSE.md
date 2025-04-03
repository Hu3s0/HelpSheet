FORENSE

COMPROBAR LAS UNIDADES DE DISCO Y PARTICIONES
.............................................

sudo fdisk -l 
df -h => enseña la ruta, tamaño, esta bien ademas de fdisk

COPIA CLONADO O IMAGEN DE DISCO
...............................

sudo dd if=<Ruta> of=<Ruta>
  Ej: sudo dd if=/dev/sbd1 of=/home/kali/imagenUSB.dd
  * La extension solo es obligatoria en windows en linux no

sudo dd if=/dev/sbd1 | pv | sudo ddof=/home/kali/imagenUSB.dd
  - Metiendo pv podemos ver el progreso de copiado

dc3dd (comando mejorado de dd)
sudo dc3dd if=/dev/sbd1 of=/home/kali/imagenUSB.dd

sudo dc3dd if=/dev/sbd1 of=/home/kali/imagenUSB.dd hash=md5 log=/home/kali/log_hash.txt
  - Guardamos el hash en un archivo al crear la copia

Si queremos copiarlo en un disco ponemos el of= con la ruta del disco

CALCULAR HASH
.............

  LINUX
  .....

  md5sum ImagenUSB.dd
  sha512sum ImagenUSB.dd

  WINDOWS
  .......


  MONTAR USB O DISCOS
  ...................

  mount => el comando solo muestra todas las rutas que hay montadas

  1 - Crear una carpeta
  2 - sudo mount /dev/sdb1 /home/kali/usb=>(ruta donde queramos montarlo)

  umount <Ruta> => para desmontarlo

  sudo mount -o ro /dev/sdb1 => para montar el disco en formato solo lectura
  
