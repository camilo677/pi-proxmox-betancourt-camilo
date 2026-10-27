# Bitacora de sesiones

La bitacora es el registro cronologico del trabajo. Anade una entrada al final
de cada sesion y conserva las anteriores. Escribe datos comprobables y enlaza
las evidencias utiles; evita frases generales como "he trabajado en Proxmox".

## Sesion 01 - 22/9/26

- Objetivo: iniciar GitHub y la documentacion Markdown.
- Acciones realizadas: PENDIENTE: configuraciones, cambios o investigacion.
- Decisiones y motivos: PENDIENTE: eleccion realizada y alternativas valoradas.
- Pruebas y resultados: PENDIENTE: resultado esperado, comando o metodo y resultado obtenido.
- Documentos o evidencias: [Ficha inicial](00-ficha-inicial.md).
- Incidencias y solucion o estado pendiente: PENDIENTE.
- Siguiente paso: PENDIENTE: accion concreta para la proxima sesion.

## Sesion 02 - 13/10/26
- Objetivo: Analizar los requisitos de hardware y la compatibilidad del equipo físico para el despliegue del hipervisor Proxmox VE 9.2-1.
- Acciones realizadas: 
  * Extracción y análisis de especificaciones de hardware mediante los comandos del sistema (`lscpu`, `lsblk` y `systemd-detect-virt`).
  * Rellenado de la tabla de contraste entre el inventario del equipo y los requisitos oficiales de Proxmox VE 9.2-1.
  * Evaluación de componentes críticos priorizando cuello de botella, disponibilidad e integridad de datos.
  * Documentación de fuentes técnicas primarias (Proxmox Wiki, especificaciones Intel ARK y documentación de systemd).
  * Redacción del dictamen de viabilidad técnica del proyecto.
- Decisiones y motivos: 
  * Se clasificó el sistema como "apto con condiciones" al comprobarse arquitectura de 64 bits y soporte de virtualización hardware, condicionándolo a la verificación del firmware.
  * Se estableció el almacenamiento mecánico (HDD TOSHIBA) como el principal cuello de botella del nodo frente al rendimiento de CPU y RAM.
- Pruebas y resultados: 
  * Ejecución de `systemd-detect-virt`, obteniendo como resultado `none`, lo que confirma que el entorno opera de forma nativa (*bare-metal*) sin capas de virtualización previas.
- Documentos o evidencias: 
  * Ficha técnica de hardware, tabla comparativa de requisitos, análisis de componentes críticos, fuentes primarias y dictamen de viabilidad integrados en la documentación del repositorio [Ver inventario](01-inventario-hardware.md)
- Incidencias y solucion o estado pendiente: 
  * Detectado aviso de mitigación (`VMX disabled`) en la CPU; pendiente comprobación y habilitación física del soporte Intel VT-x en la BIOS/UEFI de la placa base.
- Siguiente paso: 
  * Diseñar la topología de red y el esquema de direccionamiento IP para el nodo Proxmox y los servicios del proyecto ASO.

## Sesion 03 - 20/10/16
* **Objetivo:** Definir la topología de red, el direccionamiento IP estático y los requisitos de hardware para el hipervisor Proxmox VE.
* **Acciones realizadas:**
  * Planificación de la subred de laboratorio y asignación de direcciones estáticas: nodo Proxmox (`192.168.56.100`), servidor de servicios ASO (`192.168.56.10`) y cliente de pruebas (`192.168.56.20`). *(Nota: Posteriormente adaptado al esquema de doble adaptador)*.
  * Identificación de puertos críticos de gestión (`8006/TCP` para la interfaz web, `22/TCP` para SSH).
  * Diseño del diagrama lógico de red utilizando Mermaid para documentar la interconexión de los elementos.
  * Tabla de direccionamiento inicial y diagrama de red en el documento de diseño de la práctica.

Anadir una entrada con esta estructura en cada sesion. El historial de
GitHub registra los commits; no hace falta incluir el hash del propio
commit dentro de su entrada de bitacora.

[Volver al indice](../README.md)
