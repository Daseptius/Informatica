# Guía práctica de conexión SSH a una máquina virtual

> **Nota previa:** En todos los casos es imprescindible que la VM tenga instalado y en ejecución el servidor OpenSSH. En Debian/Ubuntu: `sudo apt update && sudo apt install openssh-server`. En CentOS/RHEL: `sudo yum install -y openssh-server`. Verifica su estado con `sudo systemctl status ssh` (o `sshd`).

---

## 1. Adaptador Puente (Bridged Adapter)

### ¿Cómo funciona?
La VM se conecta directamente a la red física del host, como si fuera un equipo más dentro de la misma LAN. Obtiene una dirección IP del mismo rango que el host (por ejemplo, `192.168.1.x`) y es accesible desde cualquier máquina de esa red.

### Configuración paso a paso
1. Apaga la VM.
2. En VirtualBox, selecciona la VM → **Configuración** → **Red**.
3. En **Adaptador 1**, marca **Habilitar adaptador de red**.
4. En **Conectado a**, selecciona **Adaptador puente**.
5. En **Nombre**, elige la interfaz física del host (Wi-Fi o Ethernet). Si usas Wi-Fi, algunos adaptadores no funcionan de forma fiable en modo puente; si es el caso, cambia a NAT con reenvío de puertos.
6. Acepta y arranca la VM.
7. Dentro de la VM, obtén la IP con `ip addr show`. Busca la interfaz activa (por ejemplo `enp0s3` o `eth0`) y anota la dirección `inet`.

> **Ejemplo concreto:**
> Supongamos que el host está en `192.168.1.10` y la VM obtiene `192.168.1.52`. Desde el terminal del host (o desde cualquier equipo de la misma red) ejecutarías:
> ```bash
> ssh usuario@192.168.1.52
> ```
> Sustituye `usuario` por tu nombre de usuario en la VM. Si el servidor SSH usa el puerto por defecto (22), no necesitas indicar `-p`.

---

## 2. NAT (Network Address Translation)

### ¿Cómo funciona?
La VM comparte la IP del host para salir a Internet, pero **no es accesible directamente desde fuera**. Para permitir SSH, VirtualBox ofrece **reenvío de puertos**: el host escucha en un puerto local y redirige ese tráfico al puerto 22 de la VM.

### Configuración paso a paso
1. Apaga la VM.
2. Ve a **Configuración** → **Red** → **Adaptador 1**.
3. Selecciona **NAT** en **Conectado a**.
4. Haz clic en **Avanzado** → **Reenvío de puertos**.
5. Añade una regla con estos valores:
   * **Nombre:** SSH (o cualquiera descriptivo)
   * **Protocolo:** TCP
   * **IP del host:** `127.0.0.1` (o déjalo vacío para aceptar conexiones de cualquier interfaz del host)
   * **Puerto del host:** `2222` (o cualquier puerto libre)
   * **IP del invitado:** déjalo vacío (VirtualBox lo asigna automáticamente)
   * **Puerto del invitado:** `22`
6. Guarda y arranca la VM.
7. Verifica que SSH escucha en el puerto 22 dentro de la VM con `ss -tlnp | grep :22`.

> **Ejemplo concreto:**
> Con la regla anterior, desde el terminal del host ejecutarías:
> ```bash
> ssh -p 2222 usuario@127.0.0.1
> ```
> El tráfico llega al puerto `2222` del host y VirtualBox lo redirige al puerto `22` de la VM. Si quieres que otros equipos de la red del host también puedan conectarse, en **IP del host** puedes poner la IP del host (por ejemplo `192.168.1.10`) en lugar de `127.0.0.1`, y desde otro equipo usarías `ssh -p 2222 usuario@192.168.1.10`.

---

## 3. Red NAT (NAT Network)

### ¿Cómo funciona?
Es similar a NAT, pero **múltiples VMs comparten una red interna gestionada por VirtualBox**. Cada VM recibe una IP dentro de un rango definido por ti (por ejemplo `10.0.2.0/24`), y las VMs pueden comunicarse entre sí. El reenvío de puertos se configura a nivel de **Red NAT** (no por VM individual), lo que resulta útil cuando trabajas con varios laboratorios o grupos de VMs.

### Configuración paso a paso
1. En VirtualBox, ve a **Archivo** → **Herramientas** → **Administrador de red**.
2. Pestaña **Redes NAT** → **Crear**.
3. Asigna un nombre (ej. `MiRedNAT`) y un prefijo IPv4 (ej. `10.0.2.0/24`).
4. Haz clic en **Reenvío de puertos** y añade una regla:
   * **Nombre:** SSH-VM1
   * **Protocolo:** TCP
   * **IP del host:** `127.0.0.1`
   * **Puerto del host:** `2200`
   * **IP del invitado:** `10.0.2.10` (IP que asignarás a la VM)
   * **Puerto del invitado:** `22`
5. Acepta. Ahora ve a la VM → **Configuración** → **Red** → **Adaptador 1**.
6. En **Conectado a**, selecciona **Red NAT**.
7. En **Nombre**, elige la red que creaste (`MiRedNAT`).
8. Arranca la VM y confirma que su IP sea `10.0.2.10` (o la que hayas definido) con `ip addr show`.

> **Ejemplo concreto:**
> Con la regla anterior, desde el host:
> ```bash
> ssh -p 2200 usuario@127.0.0.1
> ```
> Si tuvieras una segunda VM en la misma Red NAT con IP `10.0.2.11` y quisieras SSH directo entre VMs (sin pasar por el host), desde la primera VM podrías ejecutar `ssh usuario@10.0.2.11` directamente, ya que comparten la misma red NAT.

---

## Resumen comparativo

| Modo | IP de la VM | Acceso desde el host | Acceso desde otros equipos de la LAN | Caso de uso típico |
| :--- | :--- | :--- | :--- | :--- |
| **Puente** | IP de la LAN física (`192.168.1.52`) | `ssh usuario@192.168.1.52` | Sí, usando la IP de la VM | Laboratorio sencillo, VM como equipo real |
| **NAT** | IP interna (`10.0.2.15`) | `ssh -p 2222 usuario@127.0.0.1` | No directamente (requiere reenvío en el host) | Uso individual, sin exposición externa |
| **Red NAT** | IP de la red NAT (`10.0.2.10`) | `ssh -p 2200 usuario@127.0.0.1` | No directamente | Varias VMs que deben comunicarse entre sí |

---

## Solución de problemas frecuentes

* **Conexión rechazada:** Verifica que `sshd` esté activo dentro de la VM (`sudo systemctl status ssh`) y que el firewall de la VM permita el puerto 22 (`sudo ufw allow 22/tcp` en Ubuntu, o `sudo firewall-cmd --add-port=22/tcp --permanent` en CentOS).
* **Puerto del host ocupado:** Si el puerto `2222` o `2200` ya está en uso en el host, elige otro (por ejemplo `2223`) y ajusta el comando SSH con `-p`.
* **Modo puente sin IP:** Algunas tarjetas Wi-Fi no admiten bridging correctamente. Si `ip addr` no muestra una IP válida, cambia a NAT con reenvío de puertos.
