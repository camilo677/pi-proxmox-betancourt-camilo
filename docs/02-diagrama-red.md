# 02-diagrama-red

## Objetivo
Diseñar la topología de red, el esquema de direccionamiento IP, los parámetros de pasarela/DNS y las medidas de aislamiento necesarias para el despliegue del entorno virtualizado en Proxmox VE 9.2-1.

## Procedimiento
1. **Definición de parámetros del nodo:** Asignación del nombre del hipervisor (`pve-betancourt`) y configuración del puente de red virtual principal (`vmbr0`) vinculado a la interfaz física del equipo.
2. **Esquema de direccionamiento IP:** Planificación de la subred de laboratorio (`192.168.1.0/24`) para alojar de forma estática el nodo, el servidor de servicios ASO y el cliente de pruebas.
3. **Definición de servicios y puertos:** Identificación de los puertos de gestión (`8006`, `22`) y establecimiento de las reglas de tráfico y aislamiento de red.

## Comandos usados

```bash
# Comprobar el nombre del hostname del sistema
hostnamectl

# Verificar las direcciones IP y el estado de las interfaces de red
ip -br addr

# Comprobar la versión exacta de Proxmox VE instalada
pveversion

# Verificar el espacio de almacenamiento disponible
df -h
```

## Evidencias

## Incidencias

## Conclusion
[Volver al indice](../README.md)
