# Guía Completa: Fundamentos de la Línea de Comandos en Linux

## 1. Conceptos Fundamentales

Para comprender a fondo el funcionamiento de la interfaz de línea de comandos (**CLI**, *Command Line Interface*) en sistemas GNU/Linux y UNIX, es crucial diferenciar tres capas que habitualmente se confunden:

### 1.1. Consola

Históricamente, la **consola** era la terminal física (pantalla y teclado) conectada directamente a un ordenador central o *mainframe*. En el ámbito informático moderno, hace referencia a las **consolas virtuales** (TTYs o *Teletypes*).

* En Linux, puedes cambiar entre consolas virtuales usando las combinaciones de teclas `Ctrl + Alt + F1` a `Ctrl + Alt + F6`.
* La TTY es una interfaz textual directa proporcionada por el propio núcleo (*kernel*) del sistema sin necesidad de un servidor gráfico.

### 1.2. Terminal (Emulador de Terminal)

Es la aplicación gráfica que emula una consola dentro del entorno de escritorio (como GNOME, KDE Plasma o XFCE). Proporciona la ventana interactiva donde el usuario escribe y lee texto.

* **Ejemplos comunes:** `GNOME Terminal`, `Konsole`, `Terminator`, `Alacritty`, `Kitty` y `Tilix`.
* **Atajo estándar:** En la mayoría de distribuciones con entorno gráfico se puede abrir mediante `Ctrl + Alt + T`.

### 1.3. Shell (Intérprete de Comandos)

El **Shell** es la capa intermedia entre el usuario (a través del emulador de terminal) y el sistema operativo (*kernel*). Lee las cadenas de texto que el usuario introduce, interpreta las órdenes, analiza variables, expande patrones y ejecuta las llamadas al sistema correspondientes.

* **Tipos de Shell comunes:**
  * **`bash` (Bourne Again Shell):** Es el estándar de facto en la gran mayoría de distribuciones GNU/Linux.
  * **`zsh` (Z Shell):** Popularizada por macOS y distribuciones como Manjaro por sus capacidades avanzadas de autocompletado y personalización de temas.
  * **`fish` (Friendly Interactive Shell):** Diseñada para ser fácil de usar sin necesidad de mucha configuración previa.
  * **`sh` (Bourne Shell):** El shell original de UNIX, utilizado por motivos de compatibilidad POSIX estricta.

---

## 2. Estructura del Sistema de Archivos (FHS)

El sistema de archivos de Linux sigue la norma **FHS** (*Filesystem Hierarchy Standard*). A diferencia de sistemas como Windows (que utilizan letras de unidad como `C:\` o `D:\`), en Linux **todo parte de una única raíz representada por el símbolo `/`**. Además, en entornos UNIX se aplica el principio fundamental: *"En Linux, todo es un archivo"* (incluidos discos duros, puertos serie, procesos y sockets).

```
/ (Raíz)
├── bin -> usr/bin
├── boot
├── dev
├── etc
├── home
│   ├── usuario1
│   └── usuario2
├── lib -> usr/lib
├── media
├── mnt
├── opt
├── proc
├── root
├── sbin -> usr/sbin
├── sys
├── tmp
├── usr
└── var
```

### Detalle de los Directorios Principales

* **`/` (Raíz / Root):** El directorio base del que cuelga toda la estructura del sistema.
* **`/home`:** Espacio personal para los usuarios del sistema. Cada usuario tiene una subcarpeta (ej. `/home/juan`) donde almacena sus documentos, descargas y archivos de configuración personal (archivos ocultos o *dotfiles*).
* **`/root`:** Es el directorio personal del superusuario o administrador (`root`). Se mantiene separado de `/home` para garantizar que el administrador pueda iniciar sesión incluso si el punto de montaje `/home` no se puede cargar.
* **`/bin` y `/usr/bin`:** Contienen los ejecutables y programas esenciales utilizables por todos los usuarios (ej. `ls`, `cd`, `cp`, `bash`). En distribuciones modernas, `/bin` es un enlace simbólico a `/usr/bin`.
* **`/sbin` y `/usr/sbin`:** Almacenan binarios y herramientas administrativas destinadas al usuario administrador para el mantenimiento del sistema (ej. `fdisk`, `iptables`, `reboot`).
* **`/etc`:** Contiene los archivos de configuración globales del sistema y de los servicios instalados. Todo el comportamiento de red, usuarios, servicios y paquetes se configura editando archivos de texto plano dentro de este directorio (ej. `/etc/passwd`, `/etc/fstab`, `/etc/ssh/sshd_config`).
* **`/var`:** Almacena datos de tamaño variable que cambian constantemente durante el funcionamiento del sistema, como registros de registro de eventos (*logs* en `/var/log`), archivos de bases de datos (`/var/lib/mysql`) y colas de correo o impresión.
* **`/tmp`:** Almacena archivos temporales creados por el sistema o por las aplicaciones. Por lo general, se limpia automáticamente en cada reinicio del equipo.
* **`/dev`:** Contiene archivos especiales de dispositivo (*device files*) que representan el hardware montado en el equipo. Por ejemplo, `/dev/sda` representa el primer disco duro y `/dev/tty1` la primera consola.
* **`/proc` y `/sys`:** Son **sistemas de archivos virtuales** generados dinámicamente en memoria RAM por el *kernel*. No ocupan espacio en el disco duro.
  * `/proc` expone información detallada sobre los procesos en ejecución y parámetros del kernel (ej. `/proc/cpuinfo`, `/proc/meminfo`).
  * `/sys` expone la estructura de dispositivos y controladores de hardware.
* **`/media` y `/mnt`:** Puntos de montaje para dispositivos de almacenamiento externos. `/media` se usa para montajes automáticos (pendrives, CDs) y `/mnt` para montajes temporales manuales por parte del administrador.
* **`/opt`:** Destinado a software adicional o de terceros que no sigue el empaquetado estándar de la distribución (ej. software comercial instalado manualmente).

---

## 3. Navegación y Manipulación de Archivos y Directorios

### 3.1. Rutas Absolutas vs. Relativas

* **Ruta Absoluta:** Especifica la ubicación exacta de un archivo o directorio desde la raíz `/`. Siempre empieza por `/`.
  * *Ejemplo:* `/home/usuario/Documentos/informe.pdf`
* **Ruta Relativa:** Especifica la ubicación partiendo del directorio de trabajo actual. No empieza por `/`.
  * *Ejemplo:* Si estás en `/home/usuario`, la ruta relativa a ese mismo archivo es `Documentos/informe.pdf`.

* **Atajos de navegación:**
  * `.` : Representa el directorio actual.
  * `..` : Representa el directorio padre (un nivel más arriba).
  * `~` : Representa el directorio personal del usuario actual (`/home/usuario`).

### 3.2. Comandos de Navegación

#### `pwd` (Print Working Directory)

Muestra la ruta absoluta del directorio donde te encuentras posicionado actualmente.

```
pwd
```

*Salida de ejemplo:*
```
/home/usuario/proyectos
```

#### `cd` (Change Directory)

Permite cambiar el directorio de trabajo actual.

* Ir a una ruta absoluta específica:
```
cd /var/log
```

* Ir a un directorio secundario (ruta relativa):
```
cd Documentos
```

* Subir un nivel jerárquico:
```
cd ..
```

* Subir dos niveles jerárquicos:
```
cd ../..
```

* Ir directamente al directorio personal del usuario (`/home/usuario`):
```
cd
```
o también:
```
cd ~
```

* Volver al directorio en el que estabas posicionado justo antes del último cambio:
```
cd -
```

### 3.3. Listado de Contenido

#### `ls` (List)

Muestra el contenido de un directorio. Por defecto lista los nombres de archivos en orden alfabético.

* **Básico:**
```
ls
```

* **Listado detallado (`-l`):**
  Muestra tipo de archivo, permisos, número de enlaces, propietario, grupo, tamaño en bytes y fecha de última modificación.
```
ls -l
```

  *Salida de ejemplo:*
```
-rw-r--r-- 1 usuario grupo  4096 nov 11 10:30 notas.txt
drwxr-xr-x 2 usuario grupo  4096 nov 11 10:35 Proyectos
```

* **Mostrar archivos ocultos (`-a`):**
  En Linux, cualquier archivo o carpeta cuyo nombre empiece por un punto `.` está oculto.
```
ls -a
```

* **Tamaño legible por humanos (`-h`):**
  Muestra los tamaños en KB, MB, GB en lugar de bytes puros (se usa junto a `-l`).
```
ls -lh
```

* **Ordenar por fecha de modificación (`-t`) y de forma inversa (`-r`):**
  Los más recientes primero:
```
ls -lt
```
  Los más recientes al final del listado:
```
ls -ltr
```

### 3.4. Creación, Copia, Movimiento y Eliminación

#### `mkdir` (Make Directory)

Crea carpetas o directorios.

* Crear un directorio en la ubicación actual:
```
mkdir fotos
```

* Crear una estructura de directorios anidados de una sola vez (`-p`):
```
mkdir -p proyectos/2026/noviembre/codigo
```

#### `touch`

Crea un archivo de texto vacío si no existe. Si ya existe, actualiza sus marcas de tiempo de acceso y modificación sin alterar su contenido.

```
touch nuevo_archivo.txt
```

#### `cp` (Copy)

Copia archivos o directorios de un origen a un destino.

* Copiar un archivo en el directorio actual con otro nombre:
```
cp archivo.txt copia_archivo.txt
```

* Copiar un archivo hacia otra ruta manteniendo su nombre:
```
cp archivo.txt /tmp/
```

* Copiar un directorio entero y todo su contenido de forma recursiva (`-r` o `-R`):
```
cp -r carpeta_origen/ /backup/carpeta_destino/
```

* Modo interactivo (`-i`) para confirmar antes de sobrescribir un archivo existente:
```
cp -i foto.jpg /media/usb/
```

#### `mv` (Move)

Sirve tanto para mover archivos/directorios entre ubicaciones como para renombrarlos.

* Renombrar un archivo en el mismo directorio:
```
mv archivo_viejo.txt archivo_nuevo.txt
```

* Mover un archivo a otra carpeta:
```
mv informe.pdf Documentos/
```

* Mover y renombrar simultáneamente:
```
mv informe.pdf Documentos/informe_final_v2.pdf
```

* Mover un directorio entero (no requiere flag `-r`):
```
mv datos/ /var/backups/
```

#### `rm` (Remove)

Elimina archivos o directorios permanentemente. **Precaución:** En la línea de comandos de Linux no existe por defecto una "Papelera de reciclaje"; la eliminación es irreversible.

* Eliminar un único archivo:
```
rm notas.txt
```

* Eliminar múltiples archivos específicos:
```
rm foto1.jpg foto2.jpg documento.pdf
```

* Eliminar un directorio vacío:
```
rmdir carpeta_vacia
```

* Eliminar un directorio con todo su contenido recursivamente (`-r`):
```
rm -r carpeta_con_archivos/
```

* Forzar la eliminación sin pedir confirmación ni mostrar errores por archivos inexistentes (`-f`):
```
rm -rf carpeta_proyecto/
```

---

## 4. Gestión de Permisos y Propietarios

Linux implementa un sistema de control de acceso basado en el estándar POSIX. Cada archivo y directorio tiene asignado un **propietario**, un **grupo** y una serie de **permisos** asociados.

### 4.1. Estructura de Permisos

Al ejecutar `ls -l`, el primer bloque de 10 caracteres muestra la naturaleza y los permisos del archivo.

```
- r w x r - x r - -
│ │___| │___| │___|
│   │     │     └── Permisos para OTROS (Others / World)
│   │     └──────── Permisos para el GRUPO (Group)
│   └────────────── Permisos para el PROPIETARIO (User)
└────────────────── Tipo de archivo (- = archivo normal, d = directorio, l = enlace)
```

#### Tipos de permisos básicos y sus valores

| Permiso | Símbolo | Significado en Archivos | Significado en Directorios | Valor Octal |
| :--- | :--- | :--- | :--- | :--- |
| **Lectura** | `r` | Permite abrir y leer el contenido del archivo. | Permite listar los archivos dentro de la carpeta (`ls`). | **4** |
| **Escritura** | `w` | Permite modificar o sobrescribir el archivo. | Permite crear, borrar o renombrar archivos dentro de la carpeta. | **2** |
| **Ejecución** | `x` | Permite ejecutar el archivo (scripts, programas binarios). | Permite entrar en la carpeta (`cd`) y acceder a sus elementos. | **1** |
| **Sin Permiso** | `-` | Ningún acceso permitido. | Ningún acceso permitido. | **0** |

### 4.2. Modificación de Permisos (`chmod`)

Existen dos métodos para modificar permisos con el comando `chmod`:

#### Método 1: Notación Octal (Numérica)

Se calcula sumando los valores numéricos correspondientes ($r=4, w=2, x=1$) para cada uno de los tres grupos (User, Group, Others).

Los números de permisos que nos podemos encontrar serán los siguientes:

* 0 = — (Sin permisos)    
* 1 = –x (Solo permisos de ejecución)
* 2 = -w- (Solo permisos de escritura)
* 3 = -wx (Permisos de escritura y ejecución)
* 4 = r– (Solo permisos de lectura)
* 5 = r-x (Permisos de lectura y ejecución)
* 6 = rw- (Permisos de lectura y escritura)
* 7 = rwx (Todos los permisos)  

* **Ejemplo 1: `chmod 755 script.sh`**
    * Usuario (Propietario): $4 + 2 + 1 = 7$ (`rwx` - Control total)
    * Grupo: $4 + 0 + 1 = 5$ (`r-x` - Lectura y ejecución)
    * Otros: $4 + 0 + 1 = 5$ (`r-x` - Lectura y ejecución)

* **Ejemplo 2: `chmod 644 documento.txt`**
    * Usuario: $4 + 2 + 0 = 6$ (`rw-` - Lectura y escritura)
    * Grupo: $4 + 0 + 0 = 4$ (`r--` - Solo lectura)
    * Otros: $4 + 0 + 0 = 4$ (`r--` - Solo lectura)

* **Ejemplo 3: `chmod 700 clave_privada.pem`**
    * Usuario: $7$ (`rwx`)
    * Grupo: $0$ (`---`)
    * Otros: $0$ (`---`)

#### Método 2: Notación Simbólica

Utiliza letras para designar a quién se modifica el permiso: `u` (user), `g` (group), `o` (others), `a` (all / todos); operadores `+` (añadir), `-` (quitar), `=` (asignar exactamente); y los permisos `r`, `w`, `x`.

* Dar permiso de ejecución al propietario:
```
chmod u+x programa.sh
```

* Quitar permisos de escritura al grupo y a otros:
```
chmod go-w archivo.txt
```

* Asignar permisos idénticos de lectura a todo el mundo:
```
chmod a=r documento.txt
```

* Aplicar permisos recursivamente a todo un directorio y sus subdirectorios (`-R`):
```
chmod -R 755 /var/www/html/
```

### 4.3. Modificación de Propietario y Grupo (`chown`)

El comando `chown` (*Change Owner*) permite alterar el usuario propietario y el grupo asignado a un archivo o directorio (generalmente requiere privilegios de `root` mediante `sudo`).

* Cambiar únicamente el propietario:
```
sudo chown maria informe.docx
```

* Cambiar propietario y grupo simultáneamente:
```
sudo chown maria:desarrolladores informe.docx
```

* Cambiar solo el grupo de pertenencia:
```
sudo chown :desarrolladores informe.docx
```
O usando el comando específico:
```
sudo chgrp desarrolladores informe.docx
```

* Aplicar cambios de forma recursiva a toda una carpeta (`-R`):
```
sudo chown -R www-data:www-data /var/www/sitio_web/
```

---

## 5. Entrada/Salida Estándar y Redirecciones

Todo proceso en Linux abierto en la terminal funciona con tres flujos o descriptores de archivos predeterminados (*File Descriptors* - FD):

```
                      ┌──────────────────────┐
    [0] stdin  ─────> │                      | ─────> [1] stdout (Pantalla)
  (Teclado/Input)     │   Proceso / Comando  │
                      │                      | ─────> [2] stderr (Pantalla)
                      └──────────────────────┘
```

1. **`0` - Stdin (Standard Input):** Entrada estándar por la que el comando recibe datos (por defecto, el teclado).
2. **`1` - Stdout (Standard Output):** Salida estándar donde el comando envía los resultados exitosos (por defecto, la pantalla del emulador de terminal).
3. **`2` - Stderr (Standard Error):** Salida de errores donde el comando envía mensajes de fallo o advertencia (por defecto, la pantalla).

### 5.1. Operadores de Redirección

Podemos alterar el comportamiento predeterminado mandando las salidas a archivos en lugar de a la pantalla, o leyendo desde archivos en lugar del teclado.

#### Redirigir la salida estándar (`>` y `>>`)

* **`>` (Sobrescribir):** Redirige el flujo `stdout` a un archivo. Si el archivo ya existe, **borra su contenido previo**. Si no existe, lo crea.
```
ls -la /home/usuario > listado_archivos.txt
```

* **`>>` (Añadir / Append):** Redirige el flujo `stdout` concatenando la nueva información al final del archivo existente, sin borrar lo anterior.
```
date >> historial_fechas.log
```

#### Redirigir la entrada estándar (`<`)

* **`<`:** Envía el contenido de un archivo como entrada directa para un comando.
```
sort < lista_desordenada.txt
```

#### Redirigir la salida de errores (`2>` y `2>>`)

* Redirigir **solo los errores** a un archivo específico sin mezclar con los datos correctos:
```
find /etc -name "*.conf" 2> errores_permisos.log
```

* **Descartar errores silenciosamente (`/dev/null`):**
  `/dev/null` es el "agujero negro" de Linux. Todo lo que se envía allí se destruye al instante.
```
grep -r "clave" /var/log/ 2> /dev/null
```

#### Redirigir todo conjuntamente (`&>` y `2>&1`)

* Capturar **tanto la salida normal como los errores** en un mismo archivo:
```
hacer_backup.sh &> salida_completa.log
```

* Sintaxis clásica equivalente (POSIX):
```
hacer_backup.sh > salida_completa.log 2>&1
```

---

## 6. Procesos, Servicios y Operadores de Concatenación

### 6.1. Gestión de Procesos

Un **proceso** es una instancia de un programa ejecutable cargado en memoria RAM. Cada proceso tiene un identificador numérico único otorgado por el sistema operativo denominado **PID** (*Process ID*).

#### `ps` (Process Status)

Muestra una instantánea de los procesos activos.

* Ver los procesos en la sesión actual:
```
ps
```

* Ver **todos** los procesos en ejecución en el sistema con información detallada (`a` = todos los usuarios, `u` = formato detallado, `x` = incluye demonios sin terminal):
```
ps aux
```

#### `top` y `htop`

Monitores de recursos interactivos en tiempo real.

* **`top`:** Herramienta nativa incluida en prácticamente cualquier sistema Linux. Muestra el uso global de CPU, RAM, tiempo de actividad y la lista ordenada de procesos. (Presionar la tecla `q` para salir).
```
top
```

* **`htop`:** Versión mejorada e intuitiva con barras de colores, soporte para ratón y desplazamientos verticales/horizontales.
```
htop
```

#### `kill` y `killall` (Enviar señales a procesos)

Permiten enviar señales del sistema a uno o varios procesos para solicitar su cierre o modificar su estado.

* **Lanzar señal de terminación limpia (`SIGTERM` - Valor 15):** Pide educadamente al proceso que guarde cambios y se cierre (donde 1234 es el PID del proceso).
```
kill 1234
```

* **Lanzar señal de terminación forzada (`SIGKILL` - Valor 9):** Mata el proceso de inmediato en el kernel. Usar solo cuando el proceso no responde a `SIGTERM`.
```
kill -9 1234
```

* **Matar por nombre de programa (`killall`):**
  Finaliza todas las instancias activas de una aplicación usando su nombre en vez de su PID.
```
killall firefox
```

### 6.2. Gestión de Servicios con `systemctl`

En la mayoría de distribuciones modernas basadas en el sistema de inicio **`systemd`**, los procesos que se ejecutan en segundo plano permanentemente se conocen como **servicios** o **demonios** (*daemons*). Se gestionan usando la herramienta `systemctl`.

* **Comprobar el estado actual de un servicio:**
```
systemctl status nginx
```

* **Iniciar un servicio detenido:**
```
sudo systemctl start nginx
```

* **Detener un servicio en ejecución:**
```
sudo systemctl stop nginx
```

* **Reiniciar un servicio (Stop + Start):**
```
sudo systemctl restart nginx
```

* **Habilitar el servicio para que arranque automáticamente al encender el equipo:**
```
sudo systemctl enable nginx
```

* **Deshabilitar el inicio automático al arrancar:**
```
sudo systemctl disable nginx
```

### 6.3. Operadores y Tuberías (*Pipes*)

Los operadores de flujo permiten encadenar múltiples comandos para realizar operaciones complejas en una sola línea de comandos (*one-liners*).

#### 1. Tubería (`|` - Pipe)

Toma el flujo de salida estándar (`stdout`) del comando situado a su izquierda y lo conecta directamente como entrada estándar (`stdin`) del comando situado a su derecha.

* Filtrar los procesos para buscar únicamente los que pertenecen al servidor web Nginx:
```
ps aux | grep nginx
```

* Ver el contenido de un archivo grande paginándolo pantalla por pantalla:
```
cat /var/log/syslog | less
```

* Contar cuántos archivos de extensión `.txt` existen en el directorio actual:
```
ls -1 *.txt | wc -l
```

#### 2. Operador Secuencial (`;`)

Ejecuta los comandos de izquierda a derecha de forma secuencial **independientemente** de si el comando anterior termina con éxito o con error.

```
echo "Iniciando proceso..." ; sleep 3 ; echo "Proceso finalizado."
```

#### 3. Operador Lógico AND (`&&`)

Ejecuta el segundo comando **únicamente si el primero tuvo éxito** (es decir, devolvió un código de salida igual a `0`).

* Solo compilará la aplicación si la carpeta fue creada sin errores:
```
mkdir build && cd build
```

* Actualizar los repositorios del sistema y, solo si no hay fallos, actualizar el software instalado:
```
sudo apt update && sudo apt upgrade -y
```

#### 4. Operador Lógico OR (`||`)

Ejecuta el segundo comando **únicamente si el primero falló** (devolvió un código de salida distinto de `0`).

* Intentar entrar en un directorio; si falla, mostrar un mensaje de error personalizado:
```
cd /var/www/app || echo "Error: El directorio especificado no existe o no se puede acceder."
```

#### 5. Ejecución en Segundo Plano (`&`)

Al colocar el símbolo `&` al final de una orden, la shell ejecuta ese proceso en segundo plano (*background*) y devuelve el control del prompt al usuario inmediatamente sin tener que esperar a que termine.

* Abrir una aplicación gráfica desvinculando la terminal activa:
```
gedit notas.txt &
```
