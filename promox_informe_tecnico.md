<div align="center">

# PROXMOX

### Informe Técnico

**Gabriel Fernando Castillo Mendieta**<br>
**Esteban Nicolás Peña Coronado**<br>
**Luis Javier Lopez Galindo**

**Docente:** Frey Alfonso Santamaría Buitrago
Ingeniero de Sistemas

**Universidad Pedagógica y Tecnológica de Colombia**<br>
Ingeniería de Sistemas y Computación<br>
Electiva IaaS y Virtualización<br>
Tunja<br>
2026

</div>

---

# VIDEO DEMOSTRATIVO

A continuación se presenta el video demostrativo de la instalación y configuración de Proxmox VE, donde se evidencia paso a paso cada uno de los procedimientos descritos en este informe técnico:

🎬 **[Ver video demostrativo en OneDrive](https://onedrive.live.com/?qt=allmyphotos&photosData=%2Fshare%2FCD2767FC1448069A%21s8be72d49c95c446dab1beb443768964d%3Fithint%3Dvideo%26e%3DSzFWaq%26migratedtospo%3Dtrue&cid=CD2767FC1448069A&id=CD2767FC1448069A%21s8be72d49c95c446dab1beb443768964d&redeem=aHR0cHM6Ly8xZHJ2Lm1zL3YvYy9jZDI3NjdmYzE0NDgwNjlhL0lRQkpMZWVMWE1sdFJLc2I2MFEzYUpaTkFTYk9ScmNBLWFCLWRzSHpyaXV5WWI0P2U9U3pGV2Fx&v=photos)**

---

# TABLA DE CONTENIDOS

- [PROXMOX](#proxmox)
    - [Informe Técnico](#informe-técnico)
- [VIDEO DEMOSTRATIVO](#video-demostrativo)
- [TABLA DE CONTENIDOS](#tabla-de-contenidos)
- [LISTA DE FIGURAS](#lista-de-figuras)
  - [1. Instalación de Proxmox VE en VirtualBox](#1-instalación-de-proxmox-ve-en-virtualbox)
    - [1.1. Requisitos previos](#11-requisitos-previos)
    - [1.2. Creación de la máquina virtual en VirtualBox](#12-creación-de-la-máquina-virtual-en-virtualbox)
    - [1.3. Ajustes importantes antes de arrancar](#13-ajustes-importantes-antes-de-arrancar)
      - [1.3.1. Procesador y aceleración](#131-procesador-y-aceleración)
      - [1.3.2. Pantalla y almacenamiento](#132-pantalla-y-almacenamiento)
      - [1.3.3. Configuración de red](#133-configuración-de-red)
    - [1.4. Instalación de Proxmox VE](#14-instalación-de-proxmox-ve)
  - [2. Acceso a la Interfaz Web (GUI)](#2-acceso-a-la-interfaz-web-gui)
    - [2.1. Primer inicio de sesión](#21-primer-inicio-de-sesión)
    - [2.2. Visión general de la interfaz](#22-visión-general-de-la-interfaz)
  - [3. Gestión de Usuarios y Permisos (RBAC)](#3-gestión-de-usuarios-y-permisos-rbac)
    - [3.1. Creación de un usuario](#31-creación-de-un-usuario)
    - [3.2. Creación de grupos y asignación de roles](#32-creación-de-grupos-y-asignación-de-roles)
    - [3.3. Verificación de permisos](#33-verificación-de-permisos)
  - [4. Creación y Gestión de Máquinas Virtuales (KVM)](#4-creación-y-gestión-de-máquinas-virtuales-kvm)
    - [4.1. Configuración general de la VM](#41-configuración-general-de-la-vm)
    - [4.2. Configuración del sistema operativo](#42-configuración-del-sistema-operativo)
    - [4.3. Configuración de hardware (CPU, memoria, disco y red)](#43-configuración-de-hardware-cpu-memoria-disco-y-red)
    - [4.4. Arranque y acceso a la consola](#44-arranque-y-acceso-a-la-consola)
  - [5. Creación y Gestión de Contenedores LXC](#5-creación-y-gestión-de-contenedores-lxc)
    - [5.1. Descarga de plantillas](#51-descarga-de-plantillas)
    - [5.2. Configuración del contenedor](#52-configuración-del-contenedor)
    - [5.3. Arranque y acceso a la consola](#53-arranque-y-acceso-a-la-consola)
  - [6. Snapshots y Copias de Seguridad](#6-snapshots-y-copias-de-seguridad)
    - [6.1. Creación y restauración de Snapshots](#61-creación-y-restauración-de-snapshots)
    - [6.2. Copias de seguridad (Backups)](#62-copias-de-seguridad-backups)
  - [7. Plantillas y Clonación de VMs](#7-plantillas-y-clonación-de-vms)
    - [7.1. Conversión de VM a plantilla](#71-conversión-de-vm-a-plantilla)
    - [7.2. Clonación: Linked Clone vs. Full Clone](#72-clonación-linked-clone-vs-full-clone)
  - [8. Gestión del Ciclo de Vida desde la CLI](#8-gestión-del-ciclo-de-vida-desde-la-cli)
    - [8.1. Comandos para máquinas virtuales (qm)](#81-comandos-para-máquinas-virtuales-qm)
    - [8.2. Comandos para contenedores (pct)](#82-comandos-para-contenedores-pct)
  - [9. Monitoreo y Firewall](#9-monitoreo-y-firewall)
    - [9.1. Monitoreo de recursos](#91-monitoreo-de-recursos)
    - [9.2. Configuración del Firewall integrado](#92-configuración-del-firewall-integrado)
  - [10. Tareas e Historial de Operaciones](#10-tareas-e-historial-de-operaciones)
  - [REFERENCIAS](#referencias)

---

# LISTA DE FIGURAS

| Figura | Descripción                                               |
| ------ | --------------------------------------------------------- |
| Fg. 1  | Creación de la máquina virtual en VirtualBox              |
| Fg. 2  | Habilitación de PAE/NX en el procesador                   |
| Fg. 3  | Configuración de paravirtualización KVM                   |
| Fg. 4  | Configuración de memoria de vídeo                         |
| Fg. 5  | Configuración de adaptador puente (Bridge)                |
| Fg. 6  | Selección del disco virtual como destino de instalación   |
| Fg. 7  | Configuración de credenciales en el instalador de Proxmox |
| Fg. 8  | Asignación de IP estática durante la instalación          |
| Fg. 9  | Creación de usuario en Proxmox VE                         |
| Fg. 10 | Creación de grupo `laboratorio`                           |
| Fg. 11 | Asignación de permisos al grupo                           |
| Fg. 12 | Vista con permisos limitados del usuario `demo@pve`       |
| Fg. 13 | Asignación de nombre e ID a la VM                         |
| Fg. 14 | Selección de imagen ISO para la VM                        |
| Fg. 15 | Configuración del sistema (BIOS/UEFI)                     |
| Fg. 16 | Configuración de CPU de la VM                             |
| Fg. 17 | Configuración de memoria RAM de la VM                     |
| Fg. 18 | Configuración de disco duro de la VM                      |
| Fg. 19 | Configuración de red de la VM                             |
| Fg. 20 | Consola noVNC de la máquina virtual                       |
| Fg. 21 | Descarga de plantilla de contenedor                       |
| Fg. 22 | Asignación de nombre y contraseña al contenedor           |
| Fg. 23 | Selección de plantilla para el contenedor                 |
| Fg. 24 | Configuración de recursos del contenedor                  |
| Fg. 25 | Consola xterm.js del contenedor LXC                       |
| Fg. 26 | Snapshot creada exitosamente                              |
| Fg. 27 | Creación de respaldo (Backup)                             |
| Fg. 28 | Conversión de VM en plantilla                             |
| Fg. 29 | Clonación desde plantilla                                 |
| Fg. 30 | Panel de monitoreo del nodo                               |
| Fg. 31 | Configuración del firewall integrado                      |
| Fg. 32 | Historial de tareas del Datacenter                        |

---

## 1. Instalación de Proxmox VE en VirtualBox

> **⚠️ Advertencia importante:** La instalación de Proxmox VE dentro de VirtualBox **no se recomienda** para uso en producción. El objetivo de Proxmox es funcionar como hipervisor **bare metal** (directamente sobre el hardware físico), aprovechando al máximo el rendimiento, la estabilidad y las capacidades de virtualización nativas. La configuración descrita en esta sección tiene **únicamente fines educativos**, para quienes no disponen de un servidor físico dedicado y desean practicar en un entorno de laboratorio. En entornos reales, Proxmox VE debe instalarse directamente en el servidor mediante la imagen ISO oficial.

En esta sección se describe el proceso completo de instalación de Proxmox Virtual Environment dentro de un entorno virtualizado con Oracle VirtualBox. Esta configuración es ideal para entornos de laboratorio y aprendizaje donde no se dispone de un servidor físico dedicado.

### 1.1. Requisitos previos

Para llevar a cabo la instalación se requieren los siguientes elementos:

- **VirtualBox** instalado (versión 6.x o 7.x).
- **ISO de Proxmox VE** descargada desde [proxmox.com](https://www.proxmox.com/en/downloads).
- Al menos **4 GB de RAM** disponibles para asignar a la máquina virtual.
- Al menos **32 GB de espacio en disco** disponibles.
- Procesador con soporte de **virtualización por hardware** (VT-x/AMD-V) habilitado en la BIOS/UEFI del equipo anfitrión.

### 1.2. Creación de la máquina virtual en VirtualBox

Para crear la máquina virtual que alojará Proxmox VE, se abre VirtualBox y se selecciona la opción `Nueva`. Los parámetros de configuración iniciales son:

| Parámetro  | Valor                                     |
| ---------- | ----------------------------------------- |
| Nombre     | `Proxmox`                                 |
| Tipo       | `Linux`                                   |
| Versión    | `Debian (64-bit)`                         |
| RAM        | Mínimo `2048 MB` (recomendado `4096 MB`)  |
| Disco duro | Mínimo `32 GB` (VDI, asignación dinámica) |

<div align="center">

![Fg. 1 — Creación de la máquina virtual en VirtualBox](images/informe_tecnico/image_1.jpeg)

**Fg. 1** — Creación de la máquina virtual en VirtualBox

</div>

### 1.3. Ajustes importantes antes de arrancar

Antes de iniciar la máquina virtual, es necesario realizar una serie de ajustes críticos en la configuración de VirtualBox para garantizar el correcto funcionamiento de Proxmox VE.

#### 1.3.1. Procesador y aceleración

En `Configuración → Sistema → Procesador` se deben realizar los siguientes cambios:

- Asignar al menos **2 CPUs** virtuales.
- Activar la opción **"Habilitar PAE/NX"**, necesaria para el kernel de Proxmox.

<div align="center">

![Fg. 2 — Habilitación de PAE/NX en el procesador](images/informe_tecnico/image_2.jpeg)

**Fg. 2** — Habilitación de PAE/NX en el procesador

</div>

En `Configuración → Sistema → Aceleración`, se configura la paravirtualización como `KVM` o `Default`, lo que permite que Proxmox aproveche las extensiones de virtualización del procesador.

<div align="center">

![Fg. 3 — Configuración de paravirtualización KVM](images/informe_tecnico/image_3.jpeg)

**Fg. 3** — Configuración de paravirtualización KVM

</div>

#### 1.3.2. Pantalla y almacenamiento

En la sección `Pantalla`, se recomienda asignar al menos **16 MB de memoria de vídeo** para el correcto renderizado de la interfaz de instalación.

<div align="center">

![Fg. 4 — Configuración de memoria de vídeo](images/informe_tecnico/image_4.jpeg)

**Fg. 4** — Configuración de memoria de vídeo

</div>

En la sección `Almacenamiento`, se debe montar la **ISO de Proxmox VE** en la unidad óptica virtual para que la VM pueda arrancar desde ella.

#### 1.3.3. Configuración de red

En `Red → Adaptador 1`, se debe seleccionar el modo **Adaptador puente (Bridge)**. Esta configuración es fundamental para que Proxmox VE obtenga una dirección IP dentro de la red local y sea accesible desde el navegador del equipo anfitrión.

<div align="center">

![Fg. 5 — Configuración de adaptador puente (Bridge)](images/informe_tecnico/image_5.jpeg)

**Fg. 5** — Configuración de adaptador puente (Bridge)

</div>

### 1.4. Instalación de Proxmox VE

Con la configuración lista, se arranca la VM y se selecciona la opción **"Install Proxmox VE"** del menú de arranque. El proceso de instalación guiado consta de los siguientes pasos:

1. **Aceptar la licencia** de uso (AGPL v3).
2. **Seleccionar el disco virtual** como destino de la instalación.

<div align="center">

![Fg. 6 — Selección del disco virtual como destino de instalación](images/informe_tecnico/image_7.jpeg)

**Fg. 6** — Selección del disco virtual como destino de instalación

</div>

3. **Configurar país, zona horaria y distribución de teclado.**
4. **Establecer la contraseña de `root`** y un correo electrónico de contacto para las notificaciones del sistema.

<div align="center">

![Fg. 7 — Configuración de credenciales en el instalador de Proxmox](images/informe_tecnico/image_6.jpeg)

**Fg. 7** — Configuración de credenciales en el instalador de Proxmox

</div>

5. **Configuración de red:** asignar una IP estática (ejemplo: `192.168.1.200/24`) con el gateway correspondiente al router de la red local.

<div align="center">

![Fg. 8 — Asignación de IP estática durante la instalación](images/informe_tecnico/image_8.jpeg)

**Fg. 8** — Asignación de IP estática durante la instalación

</div>

Finalmente, se revisa el resumen de la configuración y se procede con la instalación. Una vez completada, el sistema reinicia automáticamente y queda listo para ser administrado vía web.

---

## 2. Acceso a la Interfaz Web (GUI)

### 2.1. Primer inicio de sesión

Tras la instalación y el reinicio de Proxmox VE, la consola muestra la dirección URL para acceder a la interfaz de gestión web. Para ingresar:

1. **Abrir un navegador web** en el equipo anfitrión y navegar a `https://IP-del-servidor:8006`.
2. **Aceptar la advertencia** del certificado SSL autofirmado que presenta el navegador.
3. **Iniciar sesión** con el usuario `root`, la contraseña definida durante la instalación y el realm `Linux PAM standard authentication`.

### 2.2. Visión general de la interfaz

La interfaz web de Proxmox VE se organiza de la siguiente manera:

- **Resource Tree (panel izquierdo):** estructura jerárquica que muestra `Datacenter → Nodo → VMs/CTs`, permitiendo navegar entre los distintos recursos del clúster.
- **Panel central:** muestra información detallada del recurso seleccionado, como configuraciones, consola, tareas, logs y métricas.
- **Panel de métricas en tiempo real:** gráficas de uso de CPU, RAM, red y almacenamiento del nodo o recurso seleccionado.

---

## 3. Gestión de Usuarios y Permisos (RBAC)

Proxmox VE implementa un sistema de **Control de Acceso Basado en Roles** (RBAC, por sus siglas en inglés) que permite definir de forma granular qué acciones puede realizar cada usuario sobre los diferentes recursos de la infraestructura.

### 3.1. Creación de un usuario

1. Navegar a `Datacenter → Permissions → Users`.
2. Hacer clic en `Create` y configurar un nuevo usuario, por ejemplo `demo@pve`, seleccionando el realm `Proxmox VE Authentication`. Es importante notar que los usuarios creados bajo este realm tienen acceso exclusivo a la GUI, sin posibilidad de conexión SSH directa al nodo.

<div align="center">

![Fg. 9 — Creación de usuario en Proxmox VE](images/informe_tecnico/image_10.png)

**Fg. 9** — Creación de usuario en Proxmox VE

</div>

### 3.2. Creación de grupos y asignación de roles

3. Ir a `Datacenter → Permissions → Groups` → `Create` para crear un grupo denominado `laboratorio`.

<div align="center">

![Fg. 10 — Creación de grupo laboratorio](images/informe_tecnico/image_9.png)

**Fg. 10** — Creación de grupo `laboratorio`

</div>

4. En la pestaña `Users` del grupo recién creado, seleccionar `Add` para añadir el usuario `demo@pve` al grupo `laboratorio`.
5. Navegar a `Datacenter → Permissions → Add → Group Permission` y configurar:

| Parámetro | Valor         |
| --------- | ------------- |
| Group     | `laboratorio` |
| Path      | `/vms`        |
| Role      | `PVEVMAdmin`  |

<div align="center">

![Fg. 11 — Asignación de permisos al grupo](images/informe_tecnico/image_11.png)

**Fg. 11** — Asignación de permisos al grupo

</div>

### 3.3. Verificación de permisos

6. Para verificar que los permisos se han aplicado correctamente, se cierra la sesión del usuario `root` y se inicia sesión con el usuario `demo@pve`. Al acceder, se puede observar que la interfaz muestra únicamente los recursos y opciones correspondientes al rol asignado.

<div align="center">

![Fg. 12 — Vista con permisos limitados del usuario demo@pve](images/informe_tecnico/image_12.png)

**Fg. 12** — Vista con permisos limitados del usuario `demo@pve`

</div>

---

## 4. Creación y Gestión de Máquinas Virtuales (KVM)

Las máquinas virtuales KVM proporcionan virtualización completa de hardware, permitiendo ejecutar cualquier sistema operativo compatible con la arquitectura x86-64 de forma aislada y con acceso directo a los recursos de hardware virtualizados.

### 4.1. Configuración general de la VM

1. Hacer clic en el botón `Create VM` ubicado en la parte superior derecha de la interfaz.
2. Asignar un **VMID** (por ejemplo, `101`) y un **nombre descriptivo** (por ejemplo, `vm101-lab`).

<div align="center">

![Fg. 13 — Asignación de nombre e ID a la VM](images/informe_tecnico/image_13.png)

**Fg. 13** — Asignación de nombre e ID a la VM

</div>

### 4.2. Configuración del sistema operativo

3. En la pestaña **OS**, seleccionar la imagen ISO previamente subida al almacenamiento `ISO Images`.

<div align="center">

![Fg. 14 — Selección de imagen ISO para la VM](images/informe_tecnico/image_14.png)

**Fg. 14** — Selección de imagen ISO para la VM

</div>

4. En la pestaña **System**, dejar los valores por defecto. Se puede elegir entre `SeaBIOS` (BIOS tradicional) o `OVMF/UEFI` según las necesidades del sistema operativo invitado.

<div align="center">

![Fg. 15 — Configuración del sistema (BIOS/UEFI)](images/informe_tecnico/image_15.png)

**Fg. 15** — Configuración del sistema (BIOS/UEFI)

</div>

### 4.3. Configuración de hardware (CPU, memoria, disco y red)

5. **CPU:** asignar `2` cores virtuales. El tipo de CPU recomendado es `x86-64-v2-AES`, que proporciona un buen balance entre compatibilidad y rendimiento.

<div align="center">

![Fg. 16 — Configuración de CPU de la VM](images/informe_tecnico/image_17.png)

**Fg. 16** — Configuración de CPU de la VM

</div>

6. **Memoria:** asignar `2048 MB` de RAM. Opcionalmente, se puede activar el **Ballooning**, que permite al hipervisor gestionar dinámicamente la memoria asignada según la demanda.

<div align="center">

![Fg. 17 — Configuración de memoria RAM de la VM](images/informe_tecnico/image_18.png)

**Fg. 17** — Configuración de memoria RAM de la VM

</div>

7. **Disco duro:** configurar el bus como `SCSI` con la controladora `VirtIO SCSI single`, formato `qcow2`, almacenamiento `local-lvm` y tamaño de `20 GB`. El formato `qcow2` permite snapshots y thin provisioning.

<div align="center">

![Fg. 18 — Configuración de disco duro de la VM](images/informe_tecnico/image_16.png)

**Fg. 18** — Configuración de disco duro de la VM

</div>

8. **Red:** seleccionar el modelo `VirtIO` (paravirtualizado, mejor rendimiento) y el bridge `vmbr0` para conectar la VM a la red del nodo.

<div align="center">

![Fg. 19 — Configuración de red de la VM](images/informe_tecnico/image_19.png)

**Fg. 19** — Configuración de red de la VM

</div>

### 4.4. Arranque y acceso a la consola

9. Finalizar el asistente de creación y hacer clic en `Start` para arrancar la VM.
10. Abrir la pestaña `Console` para acceder a la VM mediante **noVNC**, un cliente de escritorio remoto integrado en el navegador que no requiere software adicional.

<div align="center">

![Fg. 20 — Consola noVNC de la máquina virtual](images/informe_tecnico/image_20.png)

**Fg. 20** — Consola noVNC de la máquina virtual

</div>

---

## 5. Creación y Gestión de Contenedores LXC

Los contenedores LXC ofrecen una alternativa ligera a las máquinas virtuales tradicionales, compartiendo el kernel del sistema anfitrión y consumiendo significativamente menos recursos. Son ideales para servicios Linux que no requieren un kernel diferente o un sistema operativo distinto.

### 5.1. Descarga de plantillas

1. Navegar al almacenamiento local → sección `CT Templates`.
2. Seleccionar y descargar la plantilla deseada, por ejemplo `debian-12-standard`, desde el repositorio oficial de Proxmox.

<div align="center">

![Fg. 21 — Descarga de plantilla de contenedor](images/informe_tecnico/image_21.png)

**Fg. 21** — Descarga de plantilla de contenedor

</div>

### 5.2. Configuración del contenedor

3. Hacer clic en `Create CT` y asignar un **CTID** (por ejemplo, `201`), un **nombre** y la **contraseña root** del contenedor.

<div align="center">

![Fg. 22 — Asignación de nombre y contraseña al contenedor](images/informe_tecnico/image_22.png)

**Fg. 22** — Asignación de nombre y contraseña al contenedor

</div>

4. Seleccionar la plantilla descargada como base del contenedor.

<div align="center">

![Fg. 23 — Selección de plantilla para el contenedor](images/informe_tecnico/image_23.png)

**Fg. 23** — Selección de plantilla para el contenedor

</div>

5. Configurar los recursos del contenedor:

| Parámetro | Valor de ejemplo                   |
| --------- | ---------------------------------- |
| Disco     | `8 GB`                             |
| CPU       | `1` core                           |
| RAM       | `512 MB`                           |
| Red       | Bridge `vmbr0`, IP estática o DHCP |

<div align="center">

![Fg. 24 — Configuración de recursos del contenedor](images/informe_tecnico/image_24.png)

**Fg. 24** — Configuración de recursos del contenedor

</div>

6. Es importante dejar activada la opción **Unprivileged container** (recomendada por seguridad). Los contenedores sin privilegios mapean los UIDs del contenedor a un rango no privilegiado del host, proporcionando una capa adicional de aislamiento.

### 5.3. Arranque y acceso a la consola

7. Finalizar la configuración y arrancar el contenedor.
8. Acceder mediante la consola **xterm.js**, que a diferencia de noVNC (basada en gráficos), es una terminal ligera orientada a línea de comandos, ideal para contenedores Linux que no tienen interfaz gráfica.

<div align="center">

![Fg. 25 — Consola xterm.js del contenedor LXC](images/informe_tecnico/image_25.png)

**Fg. 25** — Consola xterm.js del contenedor LXC

</div>

---

## 6. Snapshots y Copias de Seguridad

Las snapshots y las copias de seguridad son mecanismos fundamentales para la protección de datos y la recuperación ante fallos. Proxmox VE integra ambas funcionalidades en su interfaz web.

### 6.1. Creación y restauración de Snapshots

Las snapshots (instantáneas) capturan el estado completo de una VM o contenedor en un momento determinado, incluyendo disco, RAM y configuración.

1. Seleccionar la VM (por ejemplo, `VM101`) y navegar a la pestaña `Snapshots`.
2. Hacer clic en `Take Snapshot` y asignar un nombre descriptivo, como `antes-de-cambios`.

<div align="center">

![Fg. 26 — Snapshot creada exitosamente](images/informe_tecnico/image_28.png)

**Fg. 26** — Snapshot creada exitosamente

</div>

3. Para demostrar la capacidad de reversión, se puede realizar un cambio dentro de la VM (por ejemplo, eliminar un archivo) y luego restaurar el snapshot seleccionándolo y haciendo clic en `Rollback`. La VM volverá al estado exacto en el que se encontraba al momento de la captura.

### 6.2. Copias de seguridad (Backups)

Las copias de seguridad permiten respaldar VMs y contenedores completos para su restauración posterior, incluso en un nodo diferente.

1. Seleccionar la VM → pestaña `Backup` → `Backup Now`.
2. Elegir el modo de respaldo según las necesidades:

| Modo       | Descripción                                             | Impacto          |
| ---------- | ------------------------------------------------------- | ---------------- |
| `snapshot` | Captura en caliente usando snapshots del almacenamiento | Sin downtime     |
| `suspend`  | Pausa breve de la VM durante el respaldo                | Mínimo downtime  |
| `stop`     | Apaga la VM, respalda y vuelve a encender               | Apagado completo |

3. Seleccionar el almacenamiento destino y la compresión `zstd` (recomendada por su balance entre velocidad y ratio de compresión).

<div align="center">

![Fg. 27 — Creación de respaldo (Backup)](images/informe_tecnico/image_29.png)

**Fg. 27** — Creación de respaldo (Backup)

</div>

4. Al finalizar, se genera un archivo con extensión `.vma.zst` en el almacenamiento seleccionado, listo para ser restaurado cuando sea necesario.

---

## 7. Plantillas y Clonación de VMs

Las plantillas y la clonación permiten desplegar rápidamente nuevas máquinas virtuales a partir de configuraciones predefinidas, reduciendo significativamente el tiempo de aprovisionamiento.

### 7.1. Conversión de VM a plantilla

1. Apagar la VM de origen (por ejemplo, `VM101`).
2. Hacer clic derecho sobre la VM → `Convert to Template`. **Esta operación es irreversible:** la VM dejará de ser ejecutable y se convertirá en una imagen base para clonaciones.

<div align="center">

![Fg. 28 — Conversión de VM en plantilla](images/informe_tecnico/image_26.png)

**Fg. 28** — Conversión de VM en plantilla

</div>

### 7.2. Clonación: Linked Clone vs. Full Clone

3. Hacer clic derecho sobre la plantilla → `Clone`.

<div align="center">

![Fg. 29 — Clonación desde plantilla](images/informe_tecnico/image_27.png)

**Fg. 29** — Clonación desde plantilla

</div>

4. Se presentan dos opciones de clonación:

| Tipo             | Descripción                                                                                                                   | Uso recomendado                   |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------------- | --------------------------------- |
| **Linked Clone** | Clonación instantánea que comparte la imagen base con la plantilla. Ocupa poco espacio pero depende de la plantilla original. | Laboratorios y entornos de prueba |
| **Full Clone**   | Copia completa e independiente de todos los discos. No tiene dependencia con la plantilla.                                    | Entornos de producción            |

---

## 8. Gestión del Ciclo de Vida desde la CLI

Además de la interfaz web, Proxmox VE ofrece herramientas de línea de comandos que permiten gestionar el ciclo de vida de VMs y contenedores de forma eficiente. Para acceder, se abre una conexión SSH al nodo.

### 8.1. Comandos para máquinas virtuales (qm)

```bash
# Listar todas las VMs del nodo
qm list

# Iniciar una VM
qm start 101

# Apagar una VM de forma ordenada
qm shutdown 101

# Crear un snapshot
qm snapshot 101 snap1

# Restaurar un snapshot
qm rollback 101 snap1
```

### 8.2. Comandos para contenedores (pct)

```bash
# Listar todos los contenedores
pct list

# Iniciar un contenedor
pct start 201

# Detener un contenedor
pct stop 201

# Acceder a la consola de un contenedor
pct enter 201
```

---

## 9. Monitoreo y Firewall

### 9.1. Monitoreo de recursos

Proxmox VE proporciona un completo sistema de monitoreo integrado en la interfaz web. Navegando a `Nodo → Summary`, se pueden observar gráficas históricas y en tiempo real de:

- **CPU:** porcentaje de utilización de los procesadores del nodo.
- **RAM:** memoria usada, disponible y en caché.
- **Red:** tráfico de entrada y salida en las interfaces de red.
- **Almacenamiento:** uso de los distintos almacenamientos configurados.

<div align="center">

![Fg. 30 — Panel de monitoreo del nodo](images/informe_tecnico/image_30.png)

**Fg. 30** — Panel de monitoreo del nodo

</div>

### 9.2. Configuración del Firewall integrado

Proxmox VE incluye un firewall basado en `iptables`/`nftables` que opera en **tres niveles jerárquicos**, permitiendo políticas de seguridad granulares:

1. **Nivel Datacenter:** reglas globales que aplican a todos los nodos y VMs.
2. **Nivel Nodo:** reglas específicas para el tráfico del nodo host.
3. **Nivel VM/Contenedor:** reglas aplicadas a una instancia individual.

Para activar el firewall, navegar a `Datacenter → Firewall` y habilitarlo. Se pueden agregar reglas personalizadas o utilizar **macros predefinidas** que simplifican la configuración de servicios comunes (HTTP, HTTPS, SSH, DNS, etc.).

<div align="center">

![Fg. 31 — Configuración del firewall integrado](images/informe_tecnico/image_32.png)

**Fg. 31** — Configuración del firewall integrado

</div>

---

## 10. Tareas e Historial de Operaciones

Proxmox VE registra todas las operaciones realizadas en el clúster en un historial accesible desde `Datacenter → Tasks`, ubicado en la parte inferior de la interfaz web. Este registro incluye:

- Tipo de operación (inicio, parada, backup, migración, etc.).
- Estado de la tarea (éxito, error, en progreso).
- Fecha y hora de ejecución.
- Usuario que ejecutó la operación.
- Nodo donde se realizó la tarea.

Este historial resulta invaluable para la auditoría, el diagnóstico de problemas y el seguimiento de las actividades realizadas sobre la infraestructura.

<div align="center">

![Fg. 32 — Historial de tareas del Datacenter](images/informe_tecnico/images_31.png)

**Fg. 32** — Historial de tareas del Datacenter

</div>

---

## REFERENCIAS

- Proxmox Server Solutions GmbH. (2025). _Proxmox VE Documentation_. Disponible en: [https://pve.proxmox.com/wiki/Main_Page](https://pve.proxmox.com/wiki/Main_Page)
- Proxmox Server Solutions GmbH. (2025). _Proxmox VE Administration Guide_. Disponible en: [https://pve.proxmox.com/pve-docs/pve-admin-guide.html](https://pve.proxmox.com/pve-docs/pve-admin-guide.html)
