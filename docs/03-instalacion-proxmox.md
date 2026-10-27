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

## Incidencias
Estado de interfaz en DOWN: Tras añadir el segundo adaptador virtual en VirtualBox, la interfaz correspondiente (enp0s8) apareció inicialmente inactiva en Proxmox. Se solucionó actualizando el fichero /etc/network/interfaces para enlazar correctamente el puente vmbr0 a dicha interfaz y reiniciando el servicio de red.
## Conclusion
Se ha completado con éxito la implantación de Proxmox VE en un entorno virtualizado anidado. El hipervisor se encuentra totalmente operativo y accesible a través de su interfaz web de gestión, listo para proceder al despliegue de los contenedores y máquinas virtuales requeridas en las siguientes fases del proyecto (srv-aso y cli-linux).

[Volver al indice](../README.md)
