# Guía Práctica: Conexión HTTP a una Máquina Virtual en VirtualBox (NAT y Adaptador Puente)

## 1. Conceptos Fundamentales

Para acceder desde el navegador de nuestro equipo anfitrión (*Host*) a un servidor web **HTTP** alojado dentro de una máquina virtual (**VM**) en VirtualBox, debemos configurar correctamente su adaptador de red según el modo de conexión elegido.

### 1.1. ¿Qué es HTTP y qué puerto utiliza?

* **HTTP (*Hypertext Transfer Protocol*):** Es el protocolo estándar de transferencia web.

* **Puerto estándar:** Utiliza por defecto el puerto **80** en protocolo TCP.

* **Funcionamiento:** El servidor web dentro de la VM escucha peticiones entrantes en el puerto `80`. Para llegar hasta él desde nuestro ordenador físico, VirtualBox debe enrutar el tráfico hacia la máquina virtual.

### 1.2. Modos de Red en VirtualBox: NAT vs. Adaptador Puente

* **Modo NAT (*Network Address Translation*):** La VM tiene acceso a Internet a través del equipo anfitrión, pero queda aislada en una subred privada interna (habitualmente `10.0.2.15`). Para acceder a ella desde el anfitrión es obligatorio configurar **Reenvío de Puertos** (*Port Forwarding*).

* **Modo Adaptador Puente (*Bridged Adapter*):** VirtualBox conecta la tarjeta de red virtual directamente al router físico de tu red local. La VM obtiene una dirección IP propia dentro de tu misma red (ej. `192.168.1.45`), comportándose como un dispositivo más y permitiendo el acceso directo sin reenvío de puertos.

---

## 2. Método 1: Conexión en Modo NAT (Con Reenvío de Puertos)

Este método es ideal si no quieres que la máquina virtual sea visible para otros equipos de tu red local o si estás conectado a una red Wi-Fi pública/restringida.

### 2.1. Averiguar la IP Interna de la Máquina Virtual

Inicia sesión en tu máquina virtual y ejecuta el siguiente comando para confirmar su dirección IP interna (en VirtualBox por defecto suele ser `10.0.2.15`):

```
ip a
```

### 2.2. Configurar la Regla de Reenvío de Puertos en VirtualBox

1. Selecciona tu máquina virtual en VirtualBox y entra en **Configuración** > **Red**.

2. En la pestaña **Adaptador 1**, asegúrate de que la opción **Conectado a:** esté establecida en **NAT**.

3. Despliega el menú **Avanzadas** y haz clic en el botón **Reenvío de puertos**.

4. Pulsa el icono **`+`** (Añadir nueva regla) a la derecha e introduce los siguientes valores:

| Nombre | Protocolo | IP Anfitrión | Puerto Anfitrión | IP Invitado | Puerto Invitado |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **HTTP** | TCP | `127.0.0.1` | `8080` | `10.0.2.15` | `80` |

> **Nota sobre los puertos:** En **Puerto Anfitrión** usamos `8080` (en lugar de `80`) para evitar conflictos con otros servicios de tu sistema operativo y no necesitar permisos de administrador en el anfitrión.

5. Pulsa **Aceptar** en la ventana de reglas y vuelve a pulsar **Aceptar** para guardar la configuración de red.

### 2.3. Acceder desde el Navegador del Anfitrión

Abre el navegador web en tu ordenador físico y escribe la dirección local apuntando al puerto del anfitrión configurado:

```
http://localhost:8080
```

También puedes acceder usando la dirección de bucle local (*loopback*):

```
http://127.0.0.1:8080
```

---

## 3. Método 2: Conexión en Modo Adaptador Puente (Acceso Directo)

Este método es el más cómodo en redes domésticas o de laboratorio, ya que la máquina virtual recibe una IP directa de tu router y responde en el puerto estándar sin traducciones.

### 3.1. Activar el Adaptador Puente en VirtualBox

1. Selecciona tu máquina virtual en VirtualBox y entra en **Configuración** > **Red**.

2. En la pestaña **Adaptador 1**, cambia el desplegable **Conectado a:** a **Adaptador Puente** (*Bridged Adapter*).

3. En el campo **Nombre:**, selecciona la tarjeta de red física que estés utilizando en tu equipo anfitrión para conectarte a Internet (tu tarjeta Wi-Fi o tu cable Ethernet).

4. Pulsa **Aceptar** para aplicar los cambios.

### 3.2. Averiguar la Nueva IP Local de la Máquina Virtual

Al cambiar a Adaptador Puente, la VM solicitará una nueva IP por DHCP a tu router. Ejecuta en la terminal de la VM:

```
ip a
```

Localiza la dirección IP de tu interfaz de red (tendrá el mismo rango que tu ordenador anfitrión, por ejemplo `192.168.1.45`).

### 3.3. Acceder desde el Navegador del Anfitrión

Abre el navegador web en tu ordenador físico e introduce directamente la IP de la máquina virtual (sustituyendo `192.168.1.45` por tu IP real). Al usar el puerto estándar `80`, no hace falta escribir ningún puerto en la URL:

```
http://192.168.1.45
```

---

## 4. Comprobación Rápida y Solución de Problemas

### 4.1. Verificar la Conexión desde la Terminal del Anfitrión

Puedes comprobar rápidamente si VirtualBox está enrutando bien el tráfico HTTP ejecutando `curl` desde la terminal de tu ordenador anfitrión:

* **Si usas Modo NAT:**

```
curl -I http://localhost:8080
```

* **Si usas Modo Adaptador Puente:**

```
curl -I http://192.168.1.45
```

*Si la red en VirtualBox está bien configurada, recibirás una respuesta HTTP válida desde el servidor de tu VM (por ejemplo `HTTP/1.1 200 OK`).*

### 4.2. Errores Frecuentes

* **Error `ERR_CONNECTION_REFUSED`:** La regla de VirtualBox está bien, pero el servicio web dentro de la VM está detenido o el **Puerto Invitado** no coincide con el puerto en el que escucha tu servidor (`80`).

* **Error `ERR_CONNECTION_TIMED_OUT` en NAT:** Revisa que la **IP Invitado** en la regla de reenvío de VirtualBox coincida exactamente con la IP que muestra el comando `ip a` dentro de la VM.

* **Error `ERR_CONNECTION_TIMED_OUT` en Adaptador Puente:** Verifica que en VirtualBox hayas elegido la tarjeta de red física correcta en **Nombre:** (Wi-Fi vs. Ethernet) y que el cortafuegos de la VM permita tráfico entrante al puerto `80`.
