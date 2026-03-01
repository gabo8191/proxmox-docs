<div align="center">

# PROXMOX - Documentación

**Gabriel Fernando Castillo Mendieta**
**Esteban Nicolás Peña Coronado**
**Luis Javier Lopez Galindo**

---

**Docente:** Frey Alfonso Santamaría Buitrago — Ingeniero de Sistemas

**Universidad Pedagógica y Tecnológica de Colombia**
Facultad de Ingeniería — Escuela de Ingeniería de Sistemas y Computación
Electiva IaaS y Virtualización
Tunja, 2026

</div>

---

## Descripción

Este repositorio contiene la documentación completa sobre **Proxmox Virtual Environment (VE)**, elaborada como parte de la electiva de **Infraestructura como Servicio (IaaS) y Virtualización**. Se abordan tanto los fundamentos teóricos de la plataforma como una guía práctica paso a paso para su implementación y administración.

---

## Documentos

### Marco Teórico

> **Archivo:** [proxmox_marco_teorico.md](proxmox_marco_teorico.md)

Documento de investigación que abarca los fundamentos, arquitectura y funcionalidades de Proxmox VE. Incluye los siguientes temas principales:

- **Arquitectura y Fundamentos:** definición de Proxmox VE, clasificación de hipervisores (Tipo 1 vs Tipo 2), tecnologías de virtualización integradas (KVM y LXC), modelo de licenciamiento y productos disponibles del ecosistema Proxmox.
- **Instalación y Primeros Pasos:** requisitos de hardware, proceso de instalación desde ISO, acceso a la GUI Web, visión general de la interfaz y configuraciones iniciales.
- **Gestión de Almacenamiento:** almacenamiento local (Directorios, LVM, LVM-Thin, ZFS) y almacenamiento compartido (NFS).
- **Redes Virtuales:** interfaces físicas, VLANs, Linux Bridge y el módulo SDN de Proxmox.
- **Creación y Gestión de VMs y Contenedores:** creación de VMs KVM y contenedores LXC, plantillas, clonación (linked clone vs full clone) y gestión del ciclo de vida.
- **Continuidad y Respaldos:** snapshots, copias de seguridad con vzdump y tareas de backup programadas.
- **Seguridad y Administración:** RBAC, dominios de autenticación, GUI Web, CLI (qm/pct), firewall integrado y alta disponibilidad (HA).
- **Consolas y Conexión a Instancias:** noVNC, xterm.js y acceso SSH.
- **Monitoreo Básico:** métricas de CPU, RAM, red, almacenamiento y logs del sistema/clúster.

---

### Informe Técnico (Guía Práctica)

> **Archivo:** [promox_informe_tecnico.md](promox_informe_tecnico.md)

Guía práctica paso a paso para la administración de Proxmox VE, diseñada como material de apoyo para la sesión de laboratorio. Cubre las siguientes actividades:

- **Acceso a la GUI:** conexión al panel web por HTTPS (puerto 8006) y navegación por la interfaz.
- **Gestión de Usuarios y Permisos (RBAC):** creación de usuarios, grupos y asignación de roles.
- **Creación de una Máquina Virtual KVM:** configuración de CPU, memoria, disco, red e instalación desde ISO.
- **Creación de un Contenedor LXC:** descarga de plantillas, configuración y despliegue de contenedores no privilegiados.
- **Snapshots y Backups:** creación de instantáneas, restauración (rollback) y copias de seguridad con vzdump.
- **Plantillas y Clonación:** conversión de VMs a plantillas y clonación (linked clone y full clone).
- **Gestión del Ciclo de Vida (CLI):** comandos esenciales `qm` y `pct` para administración por terminal.
- **Monitoreo y Firewall:** visualización de métricas en tiempo real y configuración del firewall a nivel Datacenter, Nodo y VM/CT.

---

## Estructura del Repositorio

```
proxmox-docs/
├── README.md                      # Este archivo
├── proxmox_marco_teorico.md       # Marco teórico e investigación
├── promox_informe_tecnico.md      # Guía práctica de laboratorio
└── images/
    └── marco_teorico/             # Imágenes del marco teórico
```

---

## Referencias Principales

- [Proxmox VE — Documentación Oficial](https://pve.proxmox.com/pve-docs/)
- [Proxmox VE — Wiki](https://pve.proxmox.com/wiki/Main_Page)
- [Proxmox Server Solutions GmbH](https://www.proxmox.com/)

> La lista completa de referencias bibliográficas se encuentra al final del [Marco Teórico](proxmox_marco_teorico.md#referencias-bibliográficas).