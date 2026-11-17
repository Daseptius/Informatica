# Guía Práctica: Conexión HTTPS a una Máquina Virtual en VirtualBox (NAT y Adaptador Puente)

## 1. Conceptos Fundamentales

Para acceder desde el navegador de nuestro equipo anfitrión (*Host*) a un servidor web cifrado mediante **HTTPS** alojado dentro de una máquina virtual (**VM**) en VirtualBox, debemos configurar el adaptador de red del hipervisor para dirigir el tráfico seguro hacia el puerto correspondiente de la máquina virtual.

### 1.1. ¿Qué es HTTPS y qué puerto utiliza?

* **HTTPS (*Hypertext Transfer Protocol Secure*):** Es la versión segura y cifrada de HTTP mediante protocolos **TLS/SSL**.

* **Puerto estándar:** Utiliza por defecto el puerto **443** en protocolo TCP.

* **Funcionamiento:** El servidor web de la VM escucha peticiones cifradas en el puerto `443`. VirtualBox se encargará de pasar ese tráfico desde nuestro ordenador físico hasta la VM sin romper el cifrado extremo a extremo.

### 1.2. Modos de Red en VirtualBox: NAT vs. Adaptador Puente

* **Modo NAT (*Network Address Translation*):** La VM queda aislada en una red interna privada (habitualmente `10.0.2.15`). Es obligatorio crear una regla de **Reenvío de Puertos** (*Port Forwarding*) en VirtualBox para redirigir un puerto del anfitrión al puerto `443` de la VM.

* **Modo Adaptador Puente (*Bridged Adapter*):** La VM se conecta directamente a tu router físico y obtiene una IP propia dentro de tu red local (ej. `192.168.1.45`), permitiendo acceder directamente por el puerto `443` sin configurar reenvío de puertos.

---

## 2. Método 1: Conexión en Modo NAT (Con Reenvío de Puertos)

Este método permite acceder al servidor HTTPS de tu máquina virtual de forma aislada desde tu propio ordenador anfitrión.

### 2.1. Averiguar la IP Interna de la Máquina Virtual

Inicia sesión en tu máquina virtual y consulta su dirección IP interna (en VirtualBox por defecto suele ser `10.0.2.15`):

```
ip a
```

### 2.2. Configurar la Regla de Reenvío de Puertos en VirtualBox

1. Selecciona tu máquina virtual en VirtualBox y entra en **Configuración** > **Red**.

2. En la pestaña **Adaptador 1**, verifica que **Conectado a:** esté en modo **NAT**.

3. Despliega **Avanzadas** y haz clic en **Reenvío de puertos**.

4. Pulsa el icono **`+`** (Añadir nueva regla) a la derecha e introduce los siguientes parámetros:

| Nombre | Protocolo | IP Anfitrión | Puerto Anfitrión | IP Invitado | Puerto Invitado |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **HTTPS** | TCP | `127.0.0.1` | `8443` | `10.0.2.15` | `443` |

> **Nota sobre los puertos:** En **Puerto Anfitrión** se utiliza `8443` (puerto alternativo estándar para HTTPS) para evitar conflictos con otros servicios de tu equipo y no requerir privilegios de administrador.

5. Pulsa **Aceptar** en la ventana de reglas y vuelve a pulsar **Aceptar** para guardar la configuración.

### 2.3. Acceder desde el Navegador del Anfitrión

Abre el navegador web en tu equipo anfitrión y accede mediante el protocolo `https://` indicando el puerto `8443`:

```
https://localhost:8443
```

También puedes utilizar la dirección IP de bucle local (*loopback*):

```
https://127.0.0.1:8443
```

---

## 3. Método 2: Conexión en Modo Adaptador Puente (Acceso Directo)

Con este método, la máquina virtual actúa como un equipo independiente dentro de tu red local y responde directamente en el puerto estándar `443`.

### 3.1. Activar el Adaptador Puente en VirtualBox

1. Selecciona tu máquina virtual en VirtualBox y entra en **Configuración** > **Red**.

2. En la pestaña **Adaptador 1**, cambia el desplegable **Conectado a:** a **Adaptador Puente** (*Bridged Adapter*).

3. En el campo **Nombre:**, selecciona tu interfaz de red física activa en el anfitrión (tarjeta Wi-Fi o adaptador Ethernet).

4. Pulsa **Aceptar** para aplicar los cambios.

### 3.2. Averiguar la Nueva IP Local de la Máquina Virtual

Consulta la dirección IP local asignada por tu router dentro de la VM ejecutando:

```
ip a
```

Anota la dirección IP asignada (tendrá el mismo rango que tu PC físico, por ejemplo `192.168.1.45`).

### 3.3. Acceder desde el Navegador del Anfitrión

Abre el navegador web en tu equipo anfitrión y escribe `https://` seguido de la IP real de tu VM. Al utilizar el puerto estándar `443`, no es necesario escribirlo en la dirección:

```
https://192.168.1.45
```

---

## 4. Aceptación del Certificado y Solución de Problemas

### 4.1. Advertencia de Seguridad en el Navegador

Si el servidor web de tu máquina virtual utiliza un **certificado autofirmado** (habitual en entornos de prácticas y laboratorio), el navegador mostrará una pantalla de advertencia (*"La conexión no es privada"* o *"Riesgo potencial de seguridad"*):

1. Haz clic en el botón **Configuración avanzada** (o *Avanzado*).

2. Pulsa en **Continuar a localhost / IP (no seguro)** (o *Aceptar el riesgo y continuar*).

3. Una vez aceptado, visualizarás la web cargada mediante conexión cifrada **HTTPS**.

### 4.2. Comprobación Rápida desde la Terminal del Anfitrión

Puedes verificar que VirtualBox redirige correctamente el tráfico SSL/TLS ejecutando `curl -k` (la opción `-k` ignora la validación de certificados autofirmados):

* **Si usas Modo NAT:**

```
curl -kI https://localhost:8443
```

* **Si usas Modo Adaptador Puente:**

```
curl -kI https://192.168.1.45
```

### 4.3. Errores Frecuentes

* **Error `ERR_SSL_PROTOCOL_ERROR`:** Estás intentando entrar por `https://` a un puerto donde el servidor de la VM está respondiendo en HTTP plano (sin SSL activado), o viceversa.

* **Error `ERR_CONNECTION_REFUSED`:** La configuración de VirtualBox es correcta, pero el servidor dentro de la VM no está escuchando en el puerto `443`.

* **Error `ERR_CONNECTION_TIMED_OUT`:** En modo **NAT**, revisa que la **IP Invitado** (`10.0.2.15`) sea la correcta. En **Adaptador Puente**, comprueba que hayas seleccionado la tarjeta física activa en VirtualBox y que el cortafuegos de la VM permita tráfico en el puerto `443`.
