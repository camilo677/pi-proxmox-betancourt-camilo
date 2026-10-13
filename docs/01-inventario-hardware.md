# Inventario investigado y viabilidad

## Contexto de la observacion

- Fecha y sistema utilizado: 13/10/26, Lliurex.
- Modalidad, equipo anfitrion y VM VirtualBox si procede: Equipo anfitrión.
- Version de Proxmox prevista: VE 9.2-1.

## Inventario inicial

| Area | Dato observado y evidencia | Especificacion y fuente | Limite o diferencia | Justificacion |
|---|---|---|---|---|
| CPU | Modelo Intel(R) Pentium(R) CPU G4560 @ 3.50GHz, 2 núcleos físicos, 4 hilos, arquitectura x86_64. | Procesador x86_64 de 64 bits | Cumple con la arquitectura y el conjunto de instrucciones de virtualización necesario. | El procesador soporta virtualización por hardware nativa, permitiendo ejecutar máquinas virtuales completas sin emulación por software. |
| RAM | Memoria base del equipo de aula (8 GB) | Mínimo recomendado de 2 GB para el nodo base, más la memoria asignada a cada máquina virtual o contenedor | Margen disponible ajustado, suficiente para el nodo y contenedores ligeros, pero limitado para múltiples VMs pesadas | Al tratarse de un entorno de laboratorio individual, se pueden desplegar servicios ligeros y contenedores optimizando la memoria asignada para evitar la saturación del host. |
| Almacenamiento | Disco duro TOSHIBA DT01ACA050 de 465,8 GB (SATA, rotacional) con particiones locales | Almacenamiento local compatible con sistemas de ficheros ext4 o ZFS para alojar el sistema operativo, plantillas e ISOs. | Amplia capacidad de espacio disponible, aunque con menor rendimiento al tratarse de un disco mecánico | El espacio de almacenamiento es más que suficiente para albergar múltiples imágenes ISO, contenedores y discos virtuales de prácticas del proyecto. |
| Red | Interfaz de red Ethernet disponible en el equipo de aula [gestionada por el hipervisor o por host en este caso] | Al menos una interfaz de red (NIC) física para la conectividad de red y gestión del nodo | Cuenta con conectividad de red operativa unicamente en el aula, con restricciones de firewall | Permite configurar el puente de red (vmbr0) para que el nodo Proxmox y sus máquinas virtuales tengan acceso a la red local y conectividad exterior. |
| Plataforma | Sistema operativo ejecutándose de forma nativa ( bare-metal ) confirmado con [systemd-detect-virt] devolviendo [none].| Hipervisor Proxmox VE de Tipo 1 diseñado directamente sobre el hardware físico del equipo [Documentación oficial] | Sin capas de virtualización previas; entorno nativo idóneo para la instalación del hipervisor. En caso de virtualizar proxmox puede generar problemas de permisos o problemas con el hipervisor al virtualizar| Operar directamente sobre el hardware (bare-metal) garantiza el acceso directo a las extensiones de virtualización del procesador sin penalización de rendimiento anidado |

## 1. Puedo instalar Proxmox en este equipo?

Contrastar el inventario con requisitos oficiales de la version prevista.
Responder por separado y justificar con datos y fuentes:

- Puedo instalar y arrancar Proxmox: Sí, con condiciones. Los datos de la CPU obtenidos mediante lscpu confirman que el equipo cuenta con una arquitectura de 64 bits (x86_64) y el conjunto de instrucciones de virtualización por hardware Intel VT-x. La comprobación con systemd-detect-virt devolviendo none demuestra que el sistema operativo actual se ejecuta de forma nativa sobre el disco duro TOSHIBA de 465,8 GB, sin capas de virtualización previas que penalicen el rendimiento. Fuente: Requisitos oficiales de instalación de Proxmox VE.
- Puedo ejecutar el servidor ASO y el cliente del proyecto: Sí. El procesador Intel Pentium G4560 de 2 núcleos físicos y 4 hilos, junto con la capacidad de almacenamiento local disponible en el disco de más de 450 GB, cumple con los requerimientos necesarios para alojar tanto el nodo hipervisor como las máquinas virtuales o contenedores LXC invitados. Fuente: Documentación técnica y dimensionamiento de contenedores de Proxmox VE.
- Condiciones o comprobaciones pendientes: Queda pendiente realizar una comprobación física en la BIOS/UEFI del equipo para confirmar que el soporte de virtualización por hardware (Intel VT-x) está habilitado correctamente y no presenta restricciones de firmware.

## 2. Que limitaciones tendre con este hardware?

Analizar RAM, CPU, disco y red para las cargas previstas. Identificar lo
medido y lo estimado. En VirtualBox, investigar host real y VM: version,
RAM reservada al anfitrion, vCPU, VDI, espacio fisico y red local.
No se utiliza el Proxmox del centro como recurso.

### Reparto de recursos

| Elemento | CPU/vCPU | RAM | Disco | Justificacion |
|---|---|---|---|---|
| Proxmox | Host sin vCPU propias; se reserva 1 hilo de 4| 2 GiB|30 GiB | 2 GB es el mínimo oficial para el SO y sus servicios. Disco: raíz y swap con ext4/LVM-thin.|
| srv-aso |1 vCPU | 2 GiB| 30 GiB| Supuesto: servidor sin entorno gráfico. Es la VM con más carga, por eso lleva más CPU. |
| cli-linux |2 vCPU |2 GiB |30 GiB | Supuesto: cliente Linux ligero. Con 1 vCPU basta para lanzar pruebas contra el servidor.|
| Plantilla, ISOs y copias | No aplica | No aplica | 30 GiB|Estimación: ISOs ≈ 6 GiB, plantilla ≈ 10 GiB, copias ≈ 24 GiB. |
| Margen de operacion |Sin hilos extra |2 Gib | 88,4 GiB| RAM: caché, memoria compartida con la gráfica integrada y picos. Disco: crecimiento de los discos thin.|

Mostrar calculos de RAM y disco, reparto de vCPU, unidades y supuestos.

vCPU: el i3-1115G4 tiene 2 núcleos físicos y 4 hilos lógicos (SMT). Asigno 2 + 1 = 3 vCPU, así que queda 1 hilo libre (25 %) para Proxmox. Dos hilos del mismo núcleo no rinden como dos núcleos completos, por lo que la capacidad real es inferior a 4 núcleos.
RAM: 2 + 2 + 2 = 6 GiB asignados de 8 GiB nominales (75 %). Quedan 2 GiB de margen (25 %). Con todo cargado a la vez cuento toda la memoria asignada como usada, sin ballooning.
Disco: 256 GB × 10⁹ / 2³⁰ = 238,4 GiB reales. Asignado: 30 + 40 + 40 + 40 = 150 GiB. Margen: 238,4 − 150 = 88,4 GiB (37 %).
Supuestos: las necesidades de srv-aso y cli-linux son estimaciones y se ajustarán a los servicios que pida la práctica.

Explicar las cargas que pueden funcionar simultaneamente.

Cargas simultáneas: Proxmox, srv-aso y cli-linux caben a la vez en RAM y disco, y en CPU quedan con 3 vCPU sobre 4 hilos. Es el peor caso asumido: todo cargado a la vez, con la CPU al límite pero sin saturar al host. Una cuarta VM no cabe sin romper el margen de RAM, y subir las vCPU a 2 + 2 dejaría al host sin hilos propios.

Medido o estimado

Medido, pendiente de comandos: hilos (lscpu), RAM visible (free -h), tamaño real del disco (lsblk) y red (ip -br link).
Especificación de la ficha del fabricante: modelo de CPU, 8 GB soldados y 256 GB NVMe.
Estimado: el reparto de arriba y los tamaños de ISOs, plantilla y copias.

| Limitacion | Evidencia o estimacion | Consecuencia | Medida y limite que permanece |
|---|---|---|---|
| | | | |

## 3. Que componentes son mas criticos en mi servidor y por que?

Ordenar al menos tres componentes para este equipo y su servicio.
Distinguir cuello de botella, disponibilidad e integridad de los datos.

| Prioridad y componente | Funcion | Efecto si es insuficiente | Efecto si falla | Evidencia y justificacion |
|---|---|---|---|---|
| 1. Almacenamiento | Almacenar el sistema operativo Proxmox, las imágenes ISO, los discos virtuales de las VMs y los contenedores LXC. | Cuello de botella: Las operaciones de lectura/escritura serán lentas al tratarse de un disco mecánico, ralentizando el arranque de las VMs y las actualizaciones. | Pérdida total de disponibilidad e integridad: Si el disco se corrompe o falla físicamente, se pierden todos los datos del nodo, contenedores y copias de seguridad locales. | Evidencia: Salida de lsblk (TOSHIBA DT01ACA050 de 465,8 GB). Es crítico porque un fallo en el almacenamiento detiene por completo la infraestructura virtualizada. |
| 2. Memoria RAM | Proveer espacio de ejecución activo para el hipervisor Proxmox VE 9.2-1 y los recursos asignados al servidor ASO y clientes. | Cuello de botella: Saturación del sistema host si se asigna más memoria de la física disponible. | Inestabilidad del sistema: El kernel puede aplicar OOM Killer y terminar procesos críticos o apagar máquinas virtuales de forma imprevista. | Evidencia: Limitación de recursos del equipo del aula (8 GB). Es un recurso escaso al repartirse entre el nodo y los invitados. |
| 3. Procesador | Ejecutar las instrucciones del hipervisor KVM, gestionar la virtualización por hardware y procesar las cargas de trabajo de los servicios. | Cuello de botella: Los 2 núcleos físicos y 4 hilos limitarán la cantidad de máquinas virtuales simultáneas y elevarán el tiempo de respuesta bajo carga. | Bloqueo del servicio: Si el procesador entra en 100% de uso sostenido, el acceso web a Proxmox (pveproxy) dejará de responder temporalmente. | Evidencia: Salida de lscpu (Intel Pentium G4560 @ 3.50GHz, 4 hilos con vmx) |

Componente que mejoraria primero y motivo para priorizarlo frente a otro:
El Almacenamiento cambiarlo a SSD. Aunque la RAM y la CPU son importantes, el disco duro mecánico actual es el mayor cuello de botella del sistema para virtualización. Las operaciones de E/S en discos mecánicos ralentizan desproporcionadamente el arranque de contenedores y máquinas virtuales. Sustituirlo por una unidad de estado sólido mejoraría drásticamente la fluidez general del nodo Proxmox sin necesidad de cambiar el equipo completo.

## Fuentes tecnicas

Consultar al menos tres fuentes primarias, incluyendo Proxmox y
fabricante o plataforma virtual. Relacionarlas con las tres respuestas.

| Fuente y URL | Version/modelo y fecha de consulta | Afirmacion respaldada | Aplicacion al nodo |
|---|---|---|---|
| (https://pve.proxmox.com/wiki/System_Requirements) | Proxmox VE 9.2-1 Consultado: Octubre 2026 | Exige obligatoriamente un procesador de 64 bits (x86_64) con soporte de virtualización por hardware (Intel VT-x o AMD-V) y un mínimo de 2 GB de RAM para el sistema base. | Valida que el equipo de aula cumple con la arquitectura exigida y soporta el arranque nativo del hipervisor KVM. |

## Dictamen de viabilidad

Apto, apto con condiciones o no apto para la propuesta actual: PENDIENTE.

Sintetizar las tres respuestas y las condiciones pendientes de comprobar.

## Evidencias y revision docente

Enlazar salidas revisadas desde ../evidencias/. Un listado automatico no
sustituye la investigacion. Registrar la pregunta de defensa y respuesta.

[Volver al indice](../README.md)
