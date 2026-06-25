# OMI Network Design

Proyecto de diseno e implementacion de una red campus para un escenario de evento (OMI), modelado en Cisco Packet Tracer.

## Resumen

Este repositorio concentra los entregables de una practica de redes que incluye:

- Diseno logico y fisico de la topologia.
- Segmentacion por VLAN.
- Plan de direccionamiento con VLSM.
- Configuraciones de routers y switches.
- Evidencia de pruebas de conectividad.

El archivo principal de simulacion es `packet-tracer/OMI_Network.pkt`.

## Alcance Tecnico Identificado

Segun la topologia logica e imagenes del proyecto:

- Arquitectura jerarquica campus con nucleo/distribucion/acceso.
- Enlace hacia router de frontera y salida a ISP/Internet.
- Segmentacion por perfiles de usuario: IT, jueces, reporteros, entrenadores, alumnos e invitados.
- Enrutamiento inter-VLAN desde router principal con subinterfaces (router-on-a-stick).
- Servicios de infraestructura como DHCP y WLC.
- Publicacion de servicios externos en la red de Internet (DNS, Concurso, NBA, CISCO).

## Plan VLAN y VLSM

Tabla sintetizada a partir de `images/vlsm-plan.png`:

| VLAN | Nombre | Hosts estimados | Subred | Mascara | Gateway |
|---|---|---:|---|---|---|
| 40 | Entrenadores | 40 | 172.23.0.0/26 | 255.255.255.192 | 172.23.0.1 |
| 30 | Reporteros | 32 | 172.23.0.64/26 | 255.255.255.192 | 172.23.0.65 |
| 20 | Jueces | 15 | 172.23.0.128/27 | 255.255.255.224 | 172.23.0.129 |
| 10 | IT | 20 | 172.23.0.160/27 | 255.255.255.224 | 172.23.0.161 |
| 50 | Alumnos | 264+ | 172.23.4.0/23 | 255.255.254.0 | 172.23.4.1 |
| 60 | Invitados | 300+ | 172.23.6.0/23 | 255.255.254.0 | 172.23.6.1 |

## Topologia y Enlaces Relevantes

De `images/logical-topology.png`:

- Router Internet:
  - G0/0: 200.1.1.1/24
  - G0/1: 200.34.100.1/24
- Router Frontera Campus:
  - G0/0: 10.32.100.1/24
  - G0/1: 200.34.100.2/24
- Router principal del campus (`RT-1TRA`) con subinterfaces por VLAN:
  - G0/0/0.10 -> 172.23.0.161/27 (VLAN 10 IT)
  - G0/0/0.20 -> 172.23.0.129/27 (VLAN 20 Jueces)
  - G0/0/0.30 -> 172.23.0.65/26 (VLAN 30 Reporteros)
  - G0/0/0.40 -> 172.23.0.1/26 (VLAN 40 Entrenadores)
  - G0/0/0.50 -> 172.23.4.1/23 (VLAN 50 Alumnos)
  - G0/0/0.60 -> 172.23.6.1/23 (VLAN 60 Invitados)
  - G0/0/1 -> 10.32.100.100/24 (uplink al core/campus)
- Servicios internos identificados:
  - DHCP: 172.23.0.168
  - WLC1: 172.23.0.162
- Servicios externos publicados:
  - DNS: 200.1.1.5
  - Concurso: 200.1.1.50
  - NBA: 200.1.1.100
  - CISCO: 200.1.1.150

## Estructura del Repositorio

```
addressing/
configurations/
documentation/
images/
packet-tracer/
```

- `packet-tracer/`: simulacion de red en Packet Tracer (`OMI_Network.pkt`).
- `images/`: diagramas y capturas de evidencia.
  - `logical-topology.png`
  - `physical-topology.png`
  - `vlsm-plan.png`
  - `connectivity-test.png`
- `documentation/`: reporte tecnico y pruebas de conectividad en PDF.
  - `Technical_Report.pdf` (26 paginas)
  - `Connectivity_Tests.pdf` (43 paginas)
- `configurations/`: respaldos/exportes de configuracion por equipo de red (routers y switches).

## Dispositivos con Configuracion Adjunta

Archivos localizados en `configurations/`:

- `internet_Router-ISP.pdf`
- `fronteraCampus_Router-Borde.pdf`
- `RT-1TRA_Router-Principal.pdf`
- `1TRA_Distribucion-Menlo.pdf`
- `SW-1TRA_Distribucion-Central.pdf`
- `SW-1TRA-EDI_1erPiso.pdf`
- `SW-2TRA_Negocios.pdf`
- `SW-7TRA_PIT-7mo.pdf`
- `SW-8TRA_PIT-8vo.pdf`
- `SW-VEL-01_Velarias.pdf`
- `SW-JUECES-IT.pdf`
- `SW-Entrenadores.pdf`
- `SW-ALUMNOS1.pdf` a `SW-ALUMNOS6.pdf`

## Evidencia de Validacion

La captura `images/connectivity-test.png` muestra una prueba de comunicacion inter-VLAN exitosa (ejemplo: PC14 en VLAN 20 hacia 172.23.0.168 en VLAN 10 por DHCP), con 0% de perdida.

Adicionalmente, `documentation/Connectivity_Tests.pdf` concentra el set completo de pruebas.

## Como Revisar el Proyecto

1. Abrir `packet-tracer/OMI_Network.pkt` en Cisco Packet Tracer.
2. Comparar el escenario con `images/logical-topology.png` y `images/physical-topology.png`.
3. Verificar el direccionamiento en `images/vlsm-plan.png`.
4. Consultar `documentation/Technical_Report.pdf` para el sustento tecnico.
5. Revisar configuraciones por equipo en `configurations/`.

## Estado

Proyecto documentado y con evidencia de conectividad. El repositorio funciona como entregable tecnico de diseno y validacion de red.