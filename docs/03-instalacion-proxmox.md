# 03-instalacion-proxmox

## Objetivo
Configurar un entorno de virtualización con Proxmox VE dentro de una máquina virtual en VirtualBox sobre un sistema operativo anfitrión Linux. El despliegue debe cumplir con los requisitos del diseño de red local: doble adaptador, asignación de IP estática de gestión, y preparación del hipervisor para albergar los servicios y clientes del laboratorio.

## Procedimiento
Creación de la máquina virtual: Registro de la VM en VirtualBox de 64 bits con recursos balanceados y un disco virtual dedicado.

Configuración de red híbrida:

Adaptador 1 (NAT): Proporciona conectividad de red y salida a internet al hipervisor.

Adaptador 2 (Solo-Anfitrión / vboxnet0): Establece una red privada (192.168.56.0/24) para la comunicación directa y segura entre el equipo anfitrión y el nodo Proxmox.

Instalación del sistema: Montaje de la ISO oficial de Proxmox VE dentro del VDI de la máquina virtual.

Acceso al panel de control: Verificación del estado de las interfaces de red y acceso exitoso a la interfaz gráfica web de administración mediante HTTPS.

## Comandos usados

```bash
# Comprobar el nombre del hostname y la información general del sistema
hostnamectl

# Verificar las direcciones IP y el estado actual de las interfaces de red
ip -br addr

# Comprobar la versión exacta de Proxmox VE instalada
pveversion

# Verificar el espacio de almacenamiento disponible en las particiones
df -h
```

## Evidencias
<img width="1870" height="940" alt="image" src="https://github.com/user-attachments/assets/31d6cee3-4cfc-474a-91e0-11f9e7d1c045" />
<img width="773" height="606" alt="image" src="https://github.com/user-attachments/assets/5be2ced5-2371-48fa-b320-31d430511f35" />

## Fase 4. Configuración base del nodo
El Almacenamiento

<img width="832" height="582" alt="image" src="https://github.com/user-attachments/assets/25b0a761-ef30-4b1c-b8fd-6b60c4e7eb3b" />

| Almacenamiento | Backend | Ruta o Pool Real | Disco que lo Sostiene | Contenido Habilitado | Espacio Disponible |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **`local`** | Directory | `/var/lib/vz` | `/dev/sda3` (`pve-root`) | Importar, Respaldo, Imagen ISO, Plantilla de contenedor | ~23,8 GiB |
| **`local-lvm`** | LVM-thin | `pve/data` | `/dev/sda3` (`pve-data`) | Imagen del disco, Contenedor | ~18 GiB | ~29,9 GiB


La Red


<img width="659" height="147" alt="image" src="https://github.com/user-attachments/assets/d996d115-0fef-42b0-89de-47716652d69a" />
<img width="1085" height="137" alt="image" src="https://github.com/user-attachments/assets/0daf77fc-af81-4df1-8836-74c31e1a419d" />
<img width="690" height="249" alt="image" src="https://github.com/user-attachments/assets/ff7564ab-f541-4a1d-adcc-1971443630a7" />

Los Accesos y la Seguridad

El usuario con el que se está entrando es el root y la contraseña que elegí instalar.
El firewall interno de Proxmox no es necesario activarlo ahora mismo para esta práctica, por lo que se queda desactivado para evitar bloqueos de red innecesarios.

Reparto de Recursos (CPU, Memoria y Disco)


| Elemento | CPU / vCPU | RAM | Disco / Almacenamiento | Motivo del reparto |
| :--- | :--- | :--- | :--- | :--- |
| **Nodo Proxmox** | 2 vCPU | 4 GB | 32 GB (VDI) | Base del hipervisor y gestión web[cite: 1]. |
| **`srv-aso`** | 1 vCPU | 1.5 GB | 10 GB (`local-lvm`) | Servidor principal de red y servicios[cite: 1]. |
| **`cli-linux`** | 1 vCPU | 1 GB | 8 GB (`local-lvm`) | Cliente ligero de pruebas[cite: 1]. |
| **Plantillas, ISO y Copias** | - | - | Margen disponible | Espacio reservado para ISOs y copias de seguridad. |

## Incidencias
Estado de interfaz en DOWN: Tras añadir el segundo adaptador virtual en VirtualBox, la interfaz correspondiente (enp0s8) apareció inicialmente inactiva en Proxmox. Se solucionó actualizando el fichero /etc/network/interfaces para enlazar correctamente el puente vmbr0 a dicha interfaz y reiniciando el servicio de red.



Se reinstaló la máquina virtual de Proxmox y se cambió la configuración de la máquina, habilitando opciones como nested paging y un disco duro dinámico de 80 gb.
## Conclusion
Se ha completado con éxito la implantación de Proxmox VE en un entorno virtualizado anidado. El hipervisor se encuentra totalmente operativo y accesible a través de su interfaz web de gestión, listo para proceder al despliegue de los contenedores y máquinas virtuales requeridas en las siguientes fases del proyecto (srv-aso y cli-linux).

[Volver al indice](../README.md)
