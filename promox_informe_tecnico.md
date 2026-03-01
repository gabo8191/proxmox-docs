# Informe Tecnico

**Requisitos previos**:
- **Proxmox instalado** y accesible en `https://IP-del-servidor:8006`.
- ISO(s) subidas a `ISO Images` y plantillas disponibles en `CT Templates` (o descargables).
- Acceso `root` con la contraseña definida en la instalación.

**Introducción (acceso a la GUI)**
- **Abrir:** Navegador → `https://IP-del-servidor:8006`
- **Certificado:** Aceptar advertencia del certificado SSL autofirmado.
- **Login:** Usuario `root`, contraseña definida, seleccionar realm `Linux PAM standard authentication`.
- **Mostrar interfaz:** Resource Tree (izquierda: Datacenter → Nodo → VMs/CTs), panel de métricas en tiempo real (CPU, RAM, red, almacenamiento).

**Gestión de Usuarios y Permisos (RBAC)**
1. Ir a `Datacenter → Permissions → Users`.
2. `Create` → Crear usuario `demo@pve` (realm: `Proxmox VEAuth`). Nota: este usuario solo GUI, sin acceso SSH.
3. Ir a `Datacenter → Permissions → Groups` → `Create` → crear grupo `laboratorio`.
4. En la pestaña `Users` del grupo, `Add` → añadir `demo@pve` al grupo `laboratorio`.
5. `Datacenter → Permissions → Add → Group Permission`:
   - Group: `laboratorio`
   - Path: `/vms`
   - Role: `PVEVMAdmin`
6. Mostrar la tabla de roles disponibles: **Administrator**, **PVEAdmin**, **PVEVMAdmin**, **PVEVMUser**, **PVEAuditor**.
7. Cerrar sesión e iniciar sesión con `demo@pve` para demostrar permisos limitados en la GUI.

**Crear una Máquina Virtual (KVM)**
1. `Create VM` (botón arriba derecha).
2. Asignar `VMID` (ej. `101`) y nombre (ej. `vm101-lab`).
3. Pestaña **OS**: seleccionar la ISO subida en `ISO Images`.
4. Pestaña **System**: dejar valores por defecto (SeaBIOS o UEFI según preferencia).
5. Pestaña **CPU**: asignar `2` cores; tipo de CPU: `x86-64-v2-AES` (recomendado).
6. Pestaña **Memory**: `2048 MB`; opcional: activar Ballooning.
7. Pestaña **Hard Disk**: bus `SCSI` + controladora `VirtIO SCSI`, formato `qcow2`, almacenamiento `local-lvm`, tamaño `20 GB`.
8. Pestaña **Network**: modelo `VirtIO`, bridge `vmbr0`.
9. Finalizar y `Start` para arrancar la VM.
10. Abrir `Console` (noVNC) desde la pestaña `Console` para mostrar acceso sin cliente adicional.

**Crear un Contenedor LXC**
1. Descargar plantilla: ir a almacenamiento local → `CT Templates` → seleccionar y descargar `debian-12-standard`.
2. `Create CT`.
3. Asignar `CTID` (ej. `201`), nombre y contraseña `root`.
4. Seleccionar la plantilla descargada.
5. Configurar disco (ej. `8 GB`), CPU (`1` core), RAM (`512 MB`), red (bridge `vmbr0`, IP estática o DHCP).
6. Importante: dejar activada la opción **Unprivileged container** (recomendada).
7. Finalizar y arrancar el contenedor.
8. Acceder mediante `Console` (xterm.js) — comparar con noVNC: xterm.js es más ligera y orientada a terminales.

**Snapshots y Backups**
- Snapshots VM:
  1. Seleccionar `VM101` → pestaña `Snapshots`.
  2. `Take Snapshot` → nombre `antes-de-cambios`.
  3. Hacer un cambio dentro de la VM (ej. borrar un archivo) para demostrar reversión.
  4. Restaurar snapshot: seleccionar snapshot → `Rollback`.
- Backups:
  1. Seleccionar VM → pestaña `Backup` → `Backup Now`.
  2. Elegir modo: `snapshot` (sin downtime), `suspend` (pausa breve) o `stop` (apagado limpio).
  3. Seleccionar almacenamiento destino y compresión `zstd`.
  4. Mostrar el archivo generado `.vma.zst` en el almacenamiento.

**Convertir VM en Plantilla y Clonar**
1. Apagar `VM101`.
2. Click derecho → `Convert to Template` (operación irreversible en esa VM).
3. Click derecho en la plantilla → `Clone`.
4. Mostrar opciones de clonación:
   - **Linked Clone**: instantáneo, ocupa poco espacio, depende de la plantilla (ideal para labs).
   - **Full Clone**: copia completa e independiente (ideal para producción).

**Gestión del Ciclo de Vida (CLI)**
Abrir SSH al nodo y ejecutar comandos esenciales:

```
# VMs con qm
qm list
qm start 101
qm shutdown 101
qm snapshot 101 snap1
qm rollback 101 snap1

# Contenedores con pct
pct list
pct start 201
pct stop 201
pct enter 201
```

**Monitoreo y Firewall**
- Ir a `Nodo → Summary`: mostrar gráficas históricas de CPU, RAM, red y almacenamiento en tiempo real.
- Ir a `Datacenter → Firewall`: activarlo y resaltar los tres niveles de firewall (Datacenter → Nodo → VM/CT).
- Agregar una regla de ejemplo para bloquear/permitir un puerto usando una macro (mostrar cómo aplicar y probar).

**Tareas y Historial**
- Mostrar `Datacenter → Tasks` en la parte inferior de la GUI: historial de todas las operaciones realizadas durante la grabación.


**Referencias**
- Documentación oficial Proxmox VE: https://pve.proxmox.com/wiki/Main_Page

