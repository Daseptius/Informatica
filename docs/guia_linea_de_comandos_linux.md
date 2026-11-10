# Guía Completa: Fundamentos de la Línea de Comandos en Linux

## 1. Conceptos Fundamentales

Para comprender el funcionamiento de la interfaz de línea de comandos (**CLI**) en entornos basados en UNIX/Linux, es esencial diferenciar tres conceptos que comúnmente se suelen confundir:

* **Consola:** Históricamente, hacía referencia al dispositivo físico (teclado y pantalla) conectado directamente al equipo. En sistemas modernos, la consola virtual es el entorno de texto interactivo proporcionado por el sistema operativo.
* **Terminal:** Es la aplicación o emulador gráfico (como *GNOME Terminal*, *Konsole*, *Terminator*, *Tilix* o *Alacritty*) que permite interactuar con la consola dentro de un entorno de escritorio. Su atajo clásico de teclado en muchos entornos es `Ctrl + Alt + T`.
* **Shell:** Es el intérprete de comandos propiamente dicho. Es el programa (ej. *Bash*, *Zsh*, *Fish*) que procesa las órdenes introducidas por el usuario y las envía al núcleo (*kernel*) para su ejecución.

---

## 2. Estructura del Sistema de Archivos

En Linux, se aplica la máxima de que **«todo es un archivo»** (incluidos los dispositivos de hardware, sockets y procesos). Todos los archivos y directorios parten de una raíz común representada por una barra diagonal (`/`).

### Directorios Principales

* `/` **(Raíz):** El nodo superior de la jerarquía.
* `/bin` y `/usr/bin`: Contienen los ejecutables y comandos esenciales del sistema y de los usuarios.
* `/sbin` y `/usr/sbin`: Almacenan binarios destinados a tareas de administración del sistema.
* `/home`: Directorio donde residen las carpetas personales de los usuarios estándar.
* `/root`: Directorio personal del usuario administrador (*root*).
* `/etc`: Contiene los archivos de configuración globales del sistema y de los servicios instalados.
* `/var`: Destinado a datos variables que cambian durante el uso del sistema, como registros (*logs*), bases de datos y colas de impresión.
* `/tmp`: Almacena archivos temporales creados por aplicaciones (se borran habitualmente al reiniciar).
* `/dev`: Aloja los archivos especiales que representan los dispositivos físicos y virtuales del hardware.
* `/proc` y `/sys`: Sistemas de archivos virtuales que proporcionan información sobre el estado del kernel, procesos en ejecución y hardware.

---

## 3. Navegación y Manipulación de Archivos y Directorios

### Navegación

* **`pwd`** (*Print Working Directory*): Muestra la ruta absoluta del directorio actual.
* **`cd`** (*Change Directory*): Cambia el directorio de trabajo.
  * `cd /ruta/absoluta`: Navega a una ubicación exacta desde la raíz.
  * `cd ruta/relativa`: Navega a una ubicación partiendo del directorio actual.
  * `cd ..`: Sube un nivel en la jerarquía de directorios (directorio padre).
  * `cd ~` o `cd`: Regresa al directorio personal del usuario.
  * `cd -`: Vuelve al directorio anterior.

### Listado de Contenido

* **`ls`**: Lista el contenido de un directorio.
  * `ls -l`: Muestra un listado detallado (permisos, propietario, grupo, tamaño y fecha de modificación).
  * `ls -a`: Incluye archivos y directorios ocultos (aquellos cuyos nombres comienzan por `.`).
  * `ls -lh`: Muestra los tamaños en un formato legible para humanos (KB, MB, GB).

### Creación, Copia y Eliminación

* **`mkdir`**: Crea un nuevo directorio.
  * `mkdir -p ruta/de/directorios`: Crea estructuras de directorios anidados de forma recursiva.
* **`touch`**: Crea un archivo vacío o actualiza la marca de tiempo de un archivo existente.
* **`cp`**: Copia archivos o directorios.
  * `cp archivo.txt copia.txt`
  * `cp -r carpeta_origen/ carpeta_destino/`: Copia recursivamente directorios y su contenido.
* **`mv`**: Mueve o renombra archivos y directorios.
  * `mv viejo_nombre.txt nuevo_nombre.txt` (Renombrar)
  * `mv archivo.txt /otra/ruta/` (Mover)
* **`rm`**: Elimina archivos o directorios.
  * `rm archivo.txt`
  * `rm -r carpeta/`: Elimina un directorio y su contenido de forma recursiva.
  * `rm -rf carpeta/`: Fuerza la eliminación recursiva sin solicitar confirmación.

---

## 4. Gestión de Permisos y Propietarios

Linux utiliza un modelo de seguridad multiusuario estricto basado en permisos de acceso para tres tipos de identidades:

1. **Propietario (*User - u*)**
2. **Grupo (*Group - g*)**
3. **Otros (*Others - o*)**

### Tipos de Permisos

* **Lectura (`r`):** Valor numérico = `4`. Permite leer el contenido de un archivo o listar un directorio.
* **Escritura (`w`):** Valor numérico = `2`. Permite modificar o eliminar un archivo, o crear/borrar archivos dentro de un directorio.
* **Ejecución (`x`):** Valor numérico = `1`. Permite ejecutar un archivo (script/binario) o acceder/atravesar un directorio.

### Comandos de Modificación

* **`chmod`**: Modifica los permisos de lectura, escritura y ejecución.
  * *Notación Octal:* `chmod 755 archivo` (Propietario: `rwx` = $4+2+1=7$, Grupo: `r-x` = $4+0+1=5$, Otros: `r-x` = $4+0+1=5$).
  * *Notación Simbólica:* `chmod u+x script.sh` (Añade permiso de ejecución al propietario).
* **`chown`**: Cambia el propietario y/o grupo de un archivo o directorio.
  * `chown usuario archivo.txt`
  * `chown usuario:grupo archivo.txt`
  * `chown -R usuario:grupo directorio/` (De forma recursiva).

---

## 5. Entrada/Salida Estándar y Redirecciones

Los procesos en Linux utilizan tres descriptores de archivo estándar para el manejo de datos:

* **`0` - Entrada Estándar (*stdin*):** Fuente de entrada de datos (por defecto, el teclado).
* **`1` - Salida Estándar (*stdout*):** Destino principal del flujo de salida de datos (por defecto, la pantalla).
* **`2` - Error Estándar (*stderr*):** Destino de los mensajes de error (por defecto, la pantalla).

### Operadores de Redirección

* **`>`**: Redirige *stdout* hacia un archivo, sobrescribiendo su contenido previo.
* **`>>`**: Redirige *stdout* hacia un archivo, añadiendo el contenido al final sin borrar lo existente.
* **`<`**: Redirige la entrada estándar (*stdin*) desde un archivo hacia un comando.
* **`2>`**: Redirige específicamente el flujo de errores (*stderr*) hacia un archivo.
* **`2> /dev/null`**: Descarta los errores enviándolos al dispositivo nulo del sistema.
* **`&>`**: Redirige tanto *stdout* como *stderr* al mismo destino.

---

## 6. Procesos, Servicios y Operadores de Concatenación

### Gestión de Procesos

Un proceso es una instancia de un programa en ejecución con un identificador único denominado **PID** (*Process ID*).

* **`ps`**: Lista los procesos activos en la sesión actual.
  * `ps aux` o `ps -ef`: Muestra todos los procesos del sistema con detalles adicionales.
* **`top` / `htop`**: Proporcionan una vista dinámica e interactiva en tiempo real del uso de CPU, memoria y procesos.
* **`kill`**: Envía una señal a un proceso para finalizarlo utilizando su PID.
  * `kill PID` (Envía la señal predeterminada `SIGTERM`).
  * `kill -9 PID` (Envía `SIGKILL` para forzar el cierre inmediato).
* **`killall`**: Finaliza procesos especificando su nombre en lugar del PID.

### Gestión de Servicios

En sistemas modernos con `systemd`, los servicios en segundo plano (*daemons*) se gestionan mediante el comando `systemctl`:

* `systemctl status servicio`: Muestra el estado del servicio.
* `systemctl start servicio`: Inicia el servicio.
* `systemctl stop servicio`: Detiene el servicio.
* `systemctl restart servicio`: Reinicia el servicio.
* `systemctl enable servicio`: Habilita el servicio para que se inicie automáticamente con el sistema.

### Operadores y Tuberías (*Pipes*)

Permiten encadenar y controlar el flujo de ejecución de múltiples comandos en una sola línea (*one-liners*).

* **Tubería (`|`):** Toma la salida estándar (*stdout*) del comando de la izquierda y la envía como entrada estándar (*stdin*) al comando de la derecha.
  * *Ejemplo:* `ps aux | grep bash`
* **Operador Secuencial (`;`):** Ejecuta los comandos en orden, uno tras otro, sin importar si el anterior tuvo éxito o falló.
  * *Ejemplo:* `cmd1 ; cmd2`
* **Operador AND (`&&`):** Ejecuta el segundo comando **únicamente si** el primero se ejecutó con éxito (código de salida 0).
  * *Ejemplo:* `mkdir carpeta && cd carpeta`
* **Operador OR (`||`):** Ejecuta el segundo comando **únicamente si** el primero falló (código de salida distinto de 0).
  * *Ejemplo:* `cd carpeta || echo "La carpeta no existe"`
* **Segundo Plano (`&`):** Ejecuta el comando en segundo plano, liberando la terminal inmediatamente para seguir introduciendo órdenes.
  * *Ejemplo:* `firefox &`