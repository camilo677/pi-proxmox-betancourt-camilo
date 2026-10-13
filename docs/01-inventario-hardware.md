# Inventario investigado y viabilidad

## Contexto de la observacion

- Fecha y sistema utilizado: 13/10/26, Lliurex.
- Modalidad, equipo anfitrion y VM VirtualBox si procede: Equipo anfitrión.
- Version de Proxmox prevista: VE 9.2-1.

## Inventario inicial

| Area | Dato observado y evidencia | Especificacion y fuente | Limite o diferencia | Justificacion |
|---|---|---|---|---|
| CPU | Modelo Intel(R) Pentium(R) CPU G4560 @ 3.50GHz, 2 núcleos físicos, 4 hilos, arquitectura x86_64 | Procesador x86_64 de 64 bits | Cumple con la arquitectura y el conjunto de instrucciones de virtualización necesario. [ comando lscpu ]. | El procesador soporta virtualización por hardware nativa, permitiendo ejecutar máquinas virtuales completas sin emulación por software. |
| RAM | Memoria base del equipo de aula (8 GB) | Mínimo recomendado de 2 GB para el nodo base, más la memoria asignada a cada máquina virtual o contenedor | Margen disponible ajustado, suficiente para el nodo y contenedores ligeros, pero limitado para múltiples VMs pesadas | Al tratarse de un entorno de laboratorio individual, se pueden desplegar servicios ligeros y contenedores optimizando la memoria asignada para evitar la saturación del host. |
| Almacenamiento | | | | |
| Red | | | | |
| Plataforma | | | | |

## 1. Puedo instalar Proxmox en este equipo?

Contrastar el inventario con requisitos oficiales de la version prevista.
Responder por separado y justificar con datos y fuentes:

- Puedo instalar y arrancar Proxmox: PENDIENTE.
- Puedo ejecutar el servidor ASO y el cliente del proyecto: PENDIENTE.
- Condiciones o comprobaciones pendientes: PENDIENTE.

## 2. Que limitaciones tendre con este hardware?

Analizar RAM, CPU, disco y red para las cargas previstas. Identificar lo
medido y lo estimado. En VirtualBox, investigar host real y VM: version,
RAM reservada al anfitrion, vCPU, VDI, espacio fisico y red local.
No se utiliza el Proxmox del centro como recurso.

### Reparto de recursos

| Elemento | CPU/vCPU | RAM | Disco | Justificacion |
|---|---|---|---|---|
| Proxmox | | | | |
| srv-aso | | | | |
| cli-linux | | | | |
| Plantilla, ISOs y copias | No aplica | No aplica | | |
| Margen de operacion | | | | |

Mostrar calculos de RAM y disco, reparto de vCPU, unidades y supuestos.
Explicar las cargas que pueden funcionar simultaneamente.

| Limitacion | Evidencia o estimacion | Consecuencia | Medida y limite que permanece |
|---|---|---|---|
| | | | |

## 3. Que componentes son mas criticos en mi servidor y por que?

Ordenar al menos tres componentes para este equipo y su servicio.
Distinguir cuello de botella, disponibilidad e integridad de los datos.

| Prioridad y componente | Funcion | Efecto si es insuficiente | Efecto si falla | Evidencia y justificacion |
|---|---|---|---|---|
| 1. | | | | |
| 2. | | | | |
| 3. | | | | |

Componente que mejoraria primero y motivo para priorizarlo frente a otro:
PENDIENTE.

## Fuentes tecnicas

Consultar al menos tres fuentes primarias, incluyendo Proxmox y
fabricante o plataforma virtual. Relacionarlas con las tres respuestas.

| Fuente y URL | Version/modelo y fecha de consulta | Afirmacion respaldada | Aplicacion al nodo |
|---|---|---|---|
| | | | |

## Dictamen de viabilidad

Apto, apto con condiciones o no apto para la propuesta actual: PENDIENTE.

Sintetizar las tres respuestas y las condiciones pendientes de comprobar.

## Evidencias y revision docente

Enlazar salidas revisadas desde ../evidencias/. Un listado automatico no
sustituye la investigacion. Registrar la pregunta de defensa y respuesta.

[Volver al indice](../README.md)
