# Mi Chuletario Linux

Mi hoja guía de comandos Bash para GNU/Linux (Distros basadas en Debian y derivadas).

Copyleft Maikel Carballo - 2021.

## Índice

* [Usuarios](#usuarios)
* [Carpetas/Archivos](#carpetas/archivos)

## Usuarios

```bash
# cambiar la contraseña del usuario unix "yo-usuario"
sudo passwd yo-usuario

# iniciar sesión con el usuario unix "yo-usuario"
su - yo-usuario
```

## Carpetas/Archivos

```bash
# muestra todos los archivos detallando: permisos, grupo, usuario,
# tamaño (en bytes) y marca de tiempo de modificado
ls -la

# igual al anterior y, además, ordena por la fecha de modificación:
# el más reciente primero
ls -lat

# cambia el grupo a una carpeta
sudo chgrp -R nombre-grupo /ruta/carpeta

# encuentra carpetas vacías (a partir de la carpeta actual)
find ./ -type d -empty

# encuentra carpetas vacías (a partir de la carpeta actual) y eliminarlas
find . -type d -empty -print0 | xargs -0 rmdir

# informa acerca del proceso que escribe en nombrearchivo.log
lsof nombrearchivo.log
#ó
fuser nombrearchivo.log

# vacía el contenido de un archivo sin eliminarlo
echo "" > archivo.log

# muestra lo que se vaya escribiendo en archivo.log
tail -f  /ruta/archivo/archivo.log

# convierte un archivo xps a formato pdf
xpstopdf /ruta_del_archivo_a_convertir/miarchivo.xps
```

## Conectando con SSH

```bash
# cuando el nombre de usuario local es el mismo en host_remoto
ssh host_remoto

# especificando usuario y host
ssh usuario_remoto@host_remoto

# especificando puerto, usuario y host
ssh -p puerto usuario_remoto@host_remoto
```

## Copiando archivos remotos con SCP

```bash
# desde local a remoto
scp archivo.txt usuario@dominio.com:/ruta/destino

# desde local a remoto recursivo
scp -r /home/usuario/carpeta usuario@dominio.com:/ruta/destino

# desde remoto a local
scp usuario@dominio.com:/ruta/origen/archivo.txt Documentos
```

## Dispositivos

```bash
# informa sobre los dispositivos de bloque (discos)
lsblk
```

## Imágenes ISO

```bash
# copia una imagen iso a un pendrive:
sudo dd bs=4M if=path/to/input.iso of=/dev/sd<?> conv=fdatasync status=progress
```

## Sistema Operativo

### Gestión de Servicios/Procesos

Con `systemctl`

```bash
# iniciar un servicio
sudo systemctl start nombre-servicio

# detener un servicio
sudo systemctl stop nombre-servicio

# detiene y vuelve iniciar el servicio
sudo systemctl restart nombre-servicio

# recarga el servicio sin desconectar puertos
sudo systemctl reload nombre-servicio

# informa estaus actual del servicio
sudo systemctl status nombre-servicio

# deshabilta el arranque automático del servicio
sudo systemctl disable nombre-servicio

# habilta el arranque automático del servicio
sudo systemctl enable nombre-servicio
```

Con `init.d`

```bash
sudo /etc/init.d/nombre-script-sh [start, stop, restart, reload, status, disable, enable]
```

Con `service`

```bash
sudo service nombre-servicio [start, stop, restart, reload, status, disable, enable]
```

Listar procesos en ejecución con nombres y PID

```bash
ps -a
```

Enviar señal `kill` al proceso "nombre-proceso"

```bash
ps -ef | grep nombre-proceso
```

Matar todos los procesos asociados al puerto 8000

```bash
sudo fuser -k 8000/tcp
```

### Información del Sistema

```bash
# usando cat
cat /etc/*release

# usando inxi, más detallado (debe ser instalado)
inxi -Fxnzr
```

### Enlaces simbólicos

```bash
# crea un enlace simbólico [-s] elimina los posibles que ya existan
sudo ln -sf ruta/objetivo ruta/de/guardado/del/enlace

# actualización manual
sudo update-alternatives --set nombre /ruta/a/binario

#actualización interactiva
sudo update-alternatives --config nombre
```

### GRUB

```bash
# actualiza el gestor de arranque
sudo update-grub
```

## PostgreSQL

Configuración básica inicial:

1. Establecer la contraseña del usuario UNIX postgres

    ```bash
    sudo passwd postgres
    ```

2. Cambiar la sesión al usuario postgres

    ```bash
    su - postgres
    # o
    sudo su -l postgres
    ```

3. Establecer la contraseña del usuario DBA postgres

    ```bash
    psql -c '\password postgres'
    ```

## Servidor HTTP Apache

```bash
# habilita el módulo rewrite
sudo a2enmod rewrite

# deshabilita el módulo rewrite
sudo a2dismod rewrite

# mostrar, en salida estándar, el log en tiempo de ejecución
tail -f  /var/log/apache2/error.log
```

## PHP

```bash
# instalación con PostgreSQL
sudo apt install phpX.X phpX.X-intl phpX.X-ldap phpX.X-mbstring phpX.X-pgsql phpX.X-xml phpX.X-zip phpX.X-curl

# cambiar entre versiones:
sudo a2dismod php8.0
sudo a2enmod php7.4
sudo systemctl restart apache2
sudo update-alternatives --set php /usr/bin/php7.4

# aumentar límite de varibles procesables por $_POST:
# editar el archivo php.ini, descomentar max_input_vars y colocar el valor deseado.
```

## Archivos TAR

### Compresión

```bash
tar -cvf salida.tar /ruta/entrada

# tar en gzip
tar -cvzf salida.tar.gz /ruta/entrada

# tar en bzip2
tar -cvjf salida.tgz2 /ruta/entrada

# añadiendo varias carpetas
tar -cvf salida.tar /ruta/directorio1 /ruta/directorio2 /ruta/directorio3

# excluyendo carpetas/archivos
tar -cvf salida.tar /ruta/entrada1 – exclude = /ruta/archivo-excluido -exclude = /ruta/directorio-excluido

#excluyendo tipos de archivos
tar -cvf salida.tar /ruta/entrada1 – exclude = /ruta/ *. txt
```

### Descompresión

```bash
tar -xvf salida.tar

# gzip
tar -zxvf salida.tar.gz

# bzip2
tar -jxvf salida.tar.gz2
```

### Lectura

```bash
tar -tvf salida.tar

# gzip
tar -ztvf salida.tar.gz

# bzip2
tar -jtvf salida.tar.gz2
```

## Muon

```bash
# actualiza la base de datos de los paquetes
sudo update-apt-xapian-index
```

## Youtube

```bash
# descarga y convierte el video a audio según los parámetros del archivo config
youtube-dl --config-location /home/maikel/.config/youtube-dl/config/ url
```

## Paquetes que deberían instalarse siempre

* `hunspell-es`
* `manpages-es`
* `wspanish`
* Para LibreOffice
  * `libreoffice-help-es`
  * `hyphen-es`
  * `mythes-es`
