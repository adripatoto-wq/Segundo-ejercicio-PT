# Segundo-ejercicio-PT

# Práctica de Redes: diseño y configuración de una red corporativa con Cisco Packet Tracer
 
**Autor:** Adrian Lopez Rruiz
**Módulo:** Redes / Sistemas Informáticos
**Herramienta:** Cisco Packet Tracer
 
---
 
## 1. Título y descripción general del proyecto
 
Este proyecto implementa en Cisco Packet Tracer una infraestructura de red corporativa formada por **cinco redes principales** y una **red de servidores**, interconectadas mediante **seis routers** unidos en cadena por enlaces serie.
 
La red incluye:
 
- Una red dividida en tres subredes con **VLSM**, aislada del resto mediante **ACL**.
- Dos redes con **DHCP** configurado en el router.
- Una red mixta con **cable y Wi-Fi (WPA2-PSK)**.
- Una red con **direccionamiento estático**.
- Una red de servidores con **DNS** y **tres servidores web** accesibles por nombre de dominio.
Todas las direcciones son privadas (RFC 1918) y no hay direcciones duplicadas ni subredes solapadas.
 
### Estructura del repositorio
 
```
.
├── README.md
├── redes2.pkt          (proyecto de Packet Tracer)
├── imagenes/           (capturas usadas en este documento)
└── web/                (HTML e imágenes de las tres páginas)
    ├── albacete.html
    ├── wagner.html
    ├── dilar.html
    ├── albacete.jpg
    ├── wagner.jpg
    └── dilar.jpg
```
 
---
 
## 2. Objetivos de la práctica
 
- Diseñar una red de varias redes interconectadas por routers.
- Calcular subredes con **VLSM** a partir de una red principal.
- Configurar direcciones IPv4 **estáticas y dinámicas (DHCP)**.
- Configurar routers y switches y el **enrutamiento estático** entre redes.
- Aislar una red mediante **listas de control de acceso (ACL)**.
- Configurar una **red inalámbrica** con punto de acceso y seguridad WPA2-PSK.
- Instalar servidores **HTTP** con páginas HTML propias y un servidor **DNS** que resuelva sus dominios.
- Comprobar la conectividad y documentar los errores encontrados y sus soluciones.
---
 
## 3. Topología completa
 
![Topología completa](imagenes/01_topologia.png)
 
Cada red tiene su propio color y está identificada con su dirección de red y máscara. Los enlaces entre routers están numerados de L1 a L5.
 
---
 
## 4. Descripción de las cinco redes y los dispositivos utilizados
 
| Red | Descripción | Router | Switch | Equipos |
|---|---|---|---|---|
| **Red 1** | Tres subredes VLSM (A, B, C), aisladas por ACL, con IP estática | R1 (Cisco 2911) | SW-A, SW-B, SW-C | A-PC1 a A-PC4, B-PC1 a B-PC4, C-PC1 a C-PC4 |
| **Red 2** | LAN con DHCP en el router | R2 (Cisco 1941) | SW-R2 | R2-PC1 a R2-PC8 |
| **Red 3** | Segunda LAN con DHCP, con rango distinto al de la Red 2 | R3 (Cisco 1941) | SW-R3 | R3-PC1 a R3-PC4 |
| **Red 4** | Red mixta: 4 PC cableados con IP fija y 5 portátiles por Wi-Fi con DHCP | R4 (Cisco 1941) | SW-R4 | R4-PC1 a R4-PC4, AP-R4 (Access Point), R4-LAP1 a R4-LAP5 |
| **Red 5** | LAN con direccionamiento estático | R5 (Cisco 1941) | SW-R5 | R5-PC1 a R5-PC5 |
| **Red 6** | Red de servidores (DNS y tres webs) | R6 (Cisco 1941) | SW-R6 | SRV-DNS, SRV-ALBACETE, SRV-WAGNER, SRV-DILAR |
 
### Dispositivos utilizados
 
| Dispositivo | Modelo | Cantidad | Función |
|---|---|---|---|
| Router | Cisco 2911 | 1 | R1, con tres puertos Gigabit para las tres subredes VLSM |
| Router | Cisco 1941 | 5 | R2 a R6 |
| Módulo serie | HWIC-2T | 6 | Puertos serie para los enlaces entre routers |
| Switch | Cisco 2960 | 8 | Uno por LAN y tres en la Red 1 |
| PC | PC-PT | 33 | Puestos de trabajo (12 + 8 + 4 + 4 + 5) |
| Portátil | Laptop-PT con módulo PT-LAPTOP-NM-1W | 5 | Clientes Wi-Fi de la Red 4 |
| Punto de acceso | Access Point-PT | 1 | AP-R4, red inalámbrica de la Red 4 |
| Servidor | Server-PT | 4 | DNS y tres servidores web |
 
### Topología lógica
 
Los routers se conectan en cadena mediante enlaces serie:
 
`R1 — R2 — R6 — R3 — R4 — R5`
 
La red de servidores (R6) está en medio de la cadena para que todas las redes autorizadas lleguen a ella con pocos saltos.
 
---
 
## 5. Tabla de direccionamiento IPv4
 
### Redes y subredes
 
| Red | Dirección de red | Máscara | Puerta de enlace | Equipos | Asignación | Rango DHCP |
|---|---|---|---|---|---|---|
| Red 1, subred A | 192.168.10.0/25 | 255.255.255.128 | 192.168.10.1 | 4 PC | Estática (.10 a .13) | No usa DHCP |
| Red 1, subred B | 192.168.10.128/27 | 255.255.255.224 | 192.168.10.129 | 4 PC | Estática (.130 a .133) | No usa DHCP |
| Red 1, subred C | 192.168.10.160/29 | 255.255.255.248 | 192.168.10.161 | 4 PC | Estática (.162 a .165) | No usa DHCP |
| Red 2 | 192.168.20.0/24 | 255.255.255.0 | 192.168.20.1 | 8 PC | DHCP | 192.168.20.10 en adelante |
| Red 3 | 192.168.30.0/24 | 255.255.255.0 | 192.168.30.1 | 4 PC | DHCP | Del pool RED3, en la red 192.168.30.0/24 |
| Red 4 | 192.168.40.0/24 | 255.255.255.0 | 192.168.40.1 | 4 PC y 5 portátiles | PC estática (.10 a .13), portátiles DHCP | 192.168.40.50 en adelante |
| Red 5 | 192.168.50.0/24 | 255.255.255.0 | 192.168.50.1 | 5 PC | Estática (.10 a .14) | No usa DHCP |
| Red 6 (servidores) | 192.168.60.0/24 | 255.255.255.0 | 192.168.60.1 | 4 servidores | Estática | No usa DHCP |
 
### Servidores (Red 6, IP estática)
 
| Servidor | IP | Función |
|---|---|---|
| SRV-DNS | 192.168.60.2 | Servidor DNS |
| SRV-ALBACETE | 192.168.60.3 | Web 1, `www.albacete.es` |
| SRV-WAGNER | 192.168.60.4 | Web 2, `www.wagner.de` |
| SRV-DILAR | 192.168.60.5 | Web 3, `www.dilar.com` |
 
El servidor DNS que reciben o tienen configurado todos los equipos es **192.168.60.2**.
 
### Enlaces serie entre routers
 
| Enlace | Red | Extremo A | IP A | Extremo B | IP B | Extremo DCE |
|---|---|---|---|---|---|---|
| L1 | 10.0.0.0/30 | R1 Se0/3/0 | 10.0.0.1 | R2 Se0/1/0 | 10.0.0.2 | R1 |
| L2 | 10.0.0.4/30 | R2 Se0/1/1 | 10.0.0.5 | R6 Se0/1/0 | 10.0.0.6 | R2 |
| L3 | 10.0.0.8/30 | R6 Se0/1/1 | 10.0.0.9 | R3 Se0/1/0 | 10.0.0.10 | R6 |
| L4 | 10.0.0.12/30 | R3 Se0/1/1 | 10.0.0.13 | R4 Se0/0/0 | 10.0.0.14 | R3 |
| L5 | 10.0.0.16/30 | R4 Se0/0/1 | 10.0.0.17 | R5 Se0/1/0 | 10.0.0.18 | R4 |
 
Los nombres de los puertos serie cambian de un router a otro según el hueco en que se instaló el módulo HWIC-2T. Se comprobaron con `show ip interface brief`.
 
---
 
## 6. Cálculos de las tres subredes VLSM
 
**Red principal elegida:** `192.168.10.0/24`, una dirección privada de clase C (RFC 1918) con 254 hosts, suficiente para las tres subredes.
 
### Método
 
1. Se ordenan las subredes de **mayor a menor** número de hosts, para no solapar bloques.
2. Para cada subred se busca el menor valor de *n* tal que 2ⁿ − 2 ≥ hosts necesarios.
3. La máscara resulta de 32 − *n* bits de red.
| Subred | Hosts necesarios | Bits de host (n) | Direcciones utilizables (2ⁿ − 2) | Prefijo | Máscara |
|---|---|---|---|---|---|
| A | 100 | 7 | 126 | /25 | 255.255.255.128 |
| B | 30 | 5 | 30 | /27 | 255.255.255.224 |
| C | 6 | 3 | 6 | /29 | 255.255.255.248 |
 
Comprobaciones: 2⁷ = 128 > 100, 2⁵ = 32 y 32 − 2 = 30 hosts exactos, 2³ = 8 y 8 − 2 = 6 hosts exactos.
 
### Resultado
 
| Subred | Dirección de red | Primera IP utilizable | Última IP utilizable | Broadcast | Máscara wildcard |
|---|---|---|---|---|---|
| A | 192.168.10.0 | 192.168.10.1 | 192.168.10.126 | 192.168.10.127 | 0.0.0.127 |
| B | 192.168.10.128 | 192.168.10.129 | 192.168.10.158 | 192.168.10.159 | 0.0.0.31 |
| C | 192.168.10.160 | 192.168.10.161 | 192.168.10.166 | 192.168.10.167 | 0.0.0.7 |
 
### Reparto del bloque
 
- **Subred A:** 192.168.10.0 a 192.168.10.127 (128 direcciones).
- **Subred B:** 192.168.10.128 a 192.168.10.159 (32 direcciones).
- **Subred C:** 192.168.10.160 a 192.168.10.167 (8 direcciones).
- **Sin usar:** 192.168.10.168 a 192.168.10.255, reservado para ampliaciones.
Los bloques son contiguos y no se solapan. La puerta de enlace de cada subred es la primera dirección utilizable (`.1`, `.129` y `.161`), asignada a una interfaz distinta de R1.
 
---
 
## 7. Configuración de los routers y enrutamiento
 
### Resumen
 
- **Seis routers** unidos en cadena por enlaces serie. El extremo **DCE** de cada cable lleva `clock rate 64000`.
- **Enrutamiento estático:** cada router tiene una ruta hacia las redes que no tiene conectadas. Las redes de los extremos (R1 y R5) usan una ruta por defecto.
- **DHCP** en los routers R2, R3 y R4.
### R1 (Red 1)
 
```
hostname R1
interface GigabitEthernet0/0
 ip address 192.168.10.1 255.255.255.128
 no shutdown
interface GigabitEthernet0/1
 ip address 192.168.10.129 255.255.255.224
 no shutdown
interface GigabitEthernet0/2
 ip address 192.168.10.161 255.255.255.248
 no shutdown
interface Serial0/3/0
 ip address 10.0.0.1 255.255.255.252
 clock rate 64000
 no shutdown
ip route 0.0.0.0 0.0.0.0 10.0.0.2
```
 
(Las ACL de R1 se explican en la sección 8.)
 
### R2 (Red 2)
 
```
hostname R2
interface GigabitEthernet0/0
 ip address 192.168.20.1 255.255.255.0
 no shutdown
interface Serial0/1/0
 ip address 10.0.0.2 255.255.255.252
 no shutdown
interface Serial0/1/1
 ip address 10.0.0.5 255.255.255.252
 clock rate 64000
 no shutdown
ip route 192.168.10.0 255.255.255.0 10.0.0.1
ip route 192.168.60.0 255.255.255.0 10.0.0.6
ip route 192.168.30.0 255.255.255.0 10.0.0.6
ip route 192.168.40.0 255.255.255.0 10.0.0.6
ip route 192.168.50.0 255.255.255.0 10.0.0.6
```
 
### R6 (Red 6, servidores)
 
```
hostname R6
interface GigabitEthernet0/0
 ip address 192.168.60.1 255.255.255.0
 no shutdown
interface Serial0/1/0
 ip address 10.0.0.6 255.255.255.252
 no shutdown
interface Serial0/1/1
 ip address 10.0.0.9 255.255.255.252
 clock rate 64000
 no shutdown
ip route 192.168.10.0 255.255.255.0 10.0.0.5
ip route 192.168.20.0 255.255.255.0 10.0.0.5
ip route 192.168.30.0 255.255.255.0 10.0.0.10
ip route 192.168.40.0 255.255.255.0 10.0.0.10
ip route 192.168.50.0 255.255.255.0 10.0.0.10
```
 
### R3 (Red 3)
 
```
hostname R3
interface GigabitEthernet0/0
 ip address 192.168.30.1 255.255.255.0
 no shutdown
interface Serial0/1/0
 ip address 10.0.0.10 255.255.255.252
 no shutdown
interface Serial0/1/1
 ip address 10.0.0.13 255.255.255.252
 clock rate 64000
 no shutdown
ip route 192.168.60.0 255.255.255.0 10.0.0.9
ip route 192.168.20.0 255.255.255.0 10.0.0.9
ip route 192.168.10.0 255.255.255.0 10.0.0.9
ip route 192.168.40.0 255.255.255.0 10.0.0.14
ip route 192.168.50.0 255.255.255.0 10.0.0.14
```
 
### R4 (Red 4)
 
```
hostname R4
interface GigabitEthernet0/0
 ip address 192.168.40.1 255.255.255.0
 no shutdown
interface Serial0/0/0
 ip address 10.0.0.14 255.255.255.252
 no shutdown
interface Serial0/0/1
 ip address 10.0.0.17 255.255.255.252
 clock rate 64000
 no shutdown
ip route 192.168.60.0 255.255.255.0 10.0.0.13
ip route 192.168.20.0 255.255.255.0 10.0.0.13
ip route 192.168.30.0 255.255.255.0 10.0.0.13
ip route 192.168.10.0 255.255.255.0 10.0.0.13
ip route 192.168.50.0 255.255.255.0 10.0.0.18
```
 
### R5 (Red 5)
 
```
hostname R5
interface GigabitEthernet0/0
 ip address 192.168.50.1 255.255.255.0
 no shutdown
interface Serial0/1/0
 ip address 10.0.0.18 255.255.255.252
 no shutdown
ip route 0.0.0.0 0.0.0.0 10.0.0.17
```
 
### Resumen de rutas
 
| Router | Destino | Siguiente salto |
|---|---|---|
| R1 | Todo (ruta por defecto) | 10.0.0.2 |
| R2 | 192.168.10.0/24 | 10.0.0.1 |
| R2 | 192.168.30.0, 40.0, 50.0 y 60.0 /24 | 10.0.0.6 |
| R6 | 192.168.10.0 y 20.0 /24 | 10.0.0.5 |
| R6 | 192.168.30.0, 40.0 y 50.0 /24 | 10.0.0.10 |
| R3 | 192.168.10.0, 20.0 y 60.0 /24 | 10.0.0.9 |
| R3 | 192.168.40.0 y 50.0 /24 | 10.0.0.14 |
| R4 | 192.168.10.0, 20.0, 30.0 y 60.0 /24 | 10.0.0.13 |
| R4 | 192.168.50.0 /24 | 10.0.0.18 |
| R5 | Todo (ruta por defecto) | 10.0.0.17 |
 
![Tabla de rutas de un router](imagenes/18_show_ip_route.png)
 
---
 
## 8. Explicación de las ACL utilizadas
 
El enunciado exige que las tres subredes de la Red 1 **no se comuniquen entre sí ni con ninguna otra red**. Como R1 tiene ruta hacia el resto y los PC tienen puerta de enlace, el aislamiento se consigue con dos ACL extendidas en R1.
 
```
access-list 101 deny ip 192.168.10.0 0.0.0.255 any
access-list 101 permit ip any any
access-list 102 deny ip any 192.168.10.0 0.0.0.255
access-list 102 permit ip any any
 
interface GigabitEthernet0/0
 ip access-group 101 in
interface GigabitEthernet0/1
 ip access-group 101 in
interface GigabitEthernet0/2
 ip access-group 101 in
interface Serial0/3/0
 ip access-group 102 in
```
 
| ACL | Dónde se aplica | Sentido | Qué hace |
|---|---|---|---|
| **101** | Las tres interfaces LAN de R1 (Gi0/0, Gi0/1 y Gi0/2) | Entrada | Bloquea todo el tráfico que **sale** de la Red 1, hacia otras redes y entre sus propias subredes |
| **102** | Interfaz serie de R1 (Se0/3/0) | Entrada | Bloquea todo el tráfico que **llega** desde fuera con destino a la Red 1 |
 
### Cómo funcionan
 
- La máscara wildcard `0.0.0.255` hace coincidir cualquier dirección de `192.168.10.0` a `192.168.10.255`, es decir, toda la Red 1.
- La ACL 101 se aplica en **entrada** en cada interfaz LAN. Cualquier paquete que salga de un PC de la Red 1 hacia otra subred o hacia el exterior pasa por R1 y se descarta.
- La ACL 102 se aplica en entrada en el enlace serie. Cualquier paquete que llegue desde otra red con destino a la Red 1 se descarta.
- Cada ACL termina en `permit ip any any` para no bloquear el resto del tráfico que pase por R1.
- Entre dos PC de la **misma subred** la comunicación no pasa por el router, así que **no** la afectan las ACL.
- Un PC de la Red 1 tampoco responde a ping a su propia puerta de enlace, porque la ACL 101 bloquea ese tráfico.
![ACL en R1](imagenes/03_acl_r1.png)
 
---
 
## 9. Configuración de los servicios DHCP, DNS, HTTP y Wi-Fi
 
### DHCP (en los routers)
 
El DHCP se configura en los routers R2, R3 y R4. Cada pool entrega dirección IP, máscara, puerta de enlace y servidor DNS.
 
```
! R2
ip dhcp excluded-address 192.168.20.1 192.168.20.9
ip dhcp pool RED2
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1
 dns-server 192.168.60.2
 
! R3
ip dhcp excluded-address 192.168.30.1 192.168.30.9
ip dhcp pool RED3
 network 192.168.30.0 255.255.255.0
 default-router 192.168.30.1
 dns-server 192.168.60.2
 
! R4
ip dhcp excluded-address 192.168.40.1 192.168.40.49
ip dhcp pool RED4
 network 192.168.40.0 255.255.255.0
 default-router 192.168.40.1
 dns-server 192.168.60.2
```
 
| Router | Pool | Red | Direcciones reservadas | Entrega |
|---|---|---|---|---|
| R2 | RED2 | 192.168.20.0/24 | Primeras direcciones, para el router | IP, máscara, gateway 192.168.20.1 y DNS 192.168.60.2 |
| R3 | RED3 | 192.168.30.0/24 | Primeras direcciones, para el router | IP, máscara, gateway 192.168.30.1 y DNS 192.168.60.2 |
| R4 | RED4 | 192.168.40.0/24 | .1 a .49 (incluye los 4 PC fijos .10 a .13) | IP, máscara, gateway 192.168.40.1 y DNS 192.168.60.2 |
 
Los rangos de las Redes 2 y 3 son distintos, como exige el enunciado (192.168.20.0/24 y 192.168.30.0/24). En la Red 4 se reservan de la `.1` a la `.49` para que el DHCP no reparta las IP fijas de los PC cableados, y los portátiles reciben desde la `.50`.
 
![DHCP en la Red 2](imagenes/05_dhcp_red2.png)
 
![DHCP en la Red 3](imagenes/06_dhcp_red3.png)
 
### DNS (SRV-DNS, 192.168.60.2)
 
Servicio DNS activado, con tres registros de tipo A:
 
| Nombre | Tipo | Dirección |
|---|---|---|
| www.albacete.es | A | 192.168.60.3 |
| www.wagner.de | A | 192.168.60.4 |
| www.dilar.com | A | 192.168.60.5 |
 
Todos los clientes reciben o tienen configurado este servidor como DNS.
 
![Registros DNS](imagenes/09_dns_registros.png)
 
### HTTP (tres servidores web)
 
Cada servidor tiene una IP estática, el servicio HTTP activado y un `index.html` propio con título, breve descripción y la imagen correspondiente, importada al servidor.
 
| Servidor | IP | Dominio | Contenido |
|---|---|---|---|
| SRV-ALBACETE | 192.168.60.3 | www.albacete.es | Fotografía de Albacete |
| SRV-WAGNER | 192.168.60.4 | www.wagner.de | Fotografía de Richard Wagner |
| SRV-DILAR | 192.168.60.5 | www.dilar.com | Fotografía de Dílar (Granada) |
 
HTML de `SRV-ALBACETE` (los otros dos siguen la misma estructura con su título, texto e imagen):
 
```html
<html>
<head><title>Albacete</title></head>
<body>
<h1>Albacete</h1>
<p>Albacete es una ciudad de Castilla-La Mancha, conocida por su feria y por su tradicion en la fabricacion de navajas.</p>
<img src="albacete.jpg" width="400">
</body>
</html>
```
 
Los archivos HTML e imágenes originales están en la carpeta `web/`.
 
### Wi-Fi (Red 4)
 
| Parámetro | Valor |
|---|---|
| Punto de acceso | AP-R4, conectado a SW-R4 |
| SSID | RED4-WIFI |
| Seguridad | WPA2-PSK con cifrado AES |
| Clave | Clave2026wifi |
| Clientes | 5 portátiles (R4-LAP1 a R4-LAP5) con IP por DHCP |
| Equipos cableados | 4 PC con IP fija (192.168.40.10 a .13) |
 
Todos los equipos de la Red 4 pertenecen a la misma red local, `192.168.40.0/24`. Los portátiles llevan el módulo inalámbrico `PT-LAPTOP-NM-1W` y reciben su dirección del DHCP de R4.
 
![Configuración del punto de acceso](imagenes/07_wifi_ap.png)
 
![Configuración de un portátil](imagenes/08_wifi_portatil.png)
 
---
 
## 10. Capturas de las páginas web
 
Acceso a cada página desde el navegador de un PC de una red autorizada, usando el nombre de dominio.
 
**www.albacete.es**
 
![Web de Albacete](imagenes/10_web_albacete.png)
 
**www.wagner.de**
 
![Web de Richard Wagner](imagenes/11_web_wagner.png)
 
**www.dilar.com**
 
![Web de Dílar](imagenes/12_web_dilar.png)
 
---
 
## 11. Pruebas de conectividad y resolución de nombres
 
| Prueba | Resultado esperado | Resultado obtenido |
|---|---|---|
| Ping entre equipos de una misma red | Correcto | [COMPLETAR] |
| Ping entre redes autorizadas | Correcto | Correcto desde la Red 5 hasta los servidores (4/4, TTL 124) |
| Ping desde Red 1 hacia las otras redes | Bloqueado | [COMPLETAR] |
| Ping desde las otras redes hacia Red 1 | Bloqueado | [COMPLETAR] |
| Asignación DHCP en las Redes 2 y 3 | Correcta | [COMPLETAR] |
| Asignación DHCP a los portátiles Wi-Fi | Correcta | [COMPLETAR] |
| Conexión de los portátiles al punto de acceso | Correcta | [COMPLETAR] |
| Resolución DNS de los tres dominios | Correcta desde las redes autorizadas | [COMPLETAR] |
| Acceso HTTP a las tres páginas | Correcto desde las redes autorizadas | [COMPLETAR] |
 
### Comandos usados
 
```
ping 192.168.10.11            (misma subred de la Red 1)
ping 192.168.60.2             (desde una red autorizada hacia los servidores)
ping 192.168.60.2             (desde un PC de la Red 1: debe fallar)
ping 192.168.10.10            (desde otra red hacia la Red 1: debe fallar)
ping www.albacete.es          (resolución DNS: devuelve 192.168.60.3)
ping www.wagner.de            (devuelve 192.168.60.4)
ping www.dilar.com            (devuelve 192.168.60.5)
```
 
### Capturas
 
**Ping dentro de una misma red**
 
![Ping en la misma red](imagenes/13_ping_misma_red.png)
 
**Ping entre redes autorizadas**
 
![Ping entre redes](imagenes/14_ping_entre_redes.png)
 
**Ping desde la Red 1 hacia otras redes (bloqueado)**
 
![Ping bloqueado desde la Red 1](imagenes/15_ping_red1_bloqueado.png)
 
**Ping desde otras redes hacia la Red 1 (bloqueado)**
 
![Ping bloqueado hacia la Red 1](imagenes/16_ping_hacia_red1_bloqueado.png)
 
**Resolución de nombres con el DNS**
 
![Resolución DNS](imagenes/17_resolucion_dns.png)
 
---
 
## 12. Problemas encontrados y soluciones aplicadas
 
| Nº | Problema | Causa | Solución |
|---|---|---|---|
| 1 | `Invalid interface type and number` al configurar el puerto serie | El módulo HWIC-2T se había instalado en un hueco distinto en cada router, y el puerto no se llamaba `Serial0/0/0` | Ver el nombre real con `show ip interface brief` (por ejemplo `Serial0/1/0` o `Serial0/3/0`) y usarlo en los comandos |
| 2 | Comandos que daban error o no se aplicaban al pegarlos en la CLI | Packet Tracer se come caracteres al pegar varias líneas seguidas | Pegar los comandos en bloques pequeños, escribir a mano los más delicados y comprobar con `show running-config` |
| 3 | Un enlace serie no se activaba en R3 | Las IP de los dos puertos serie estaban cruzadas | Reasignar `10.0.0.10` a `Serial0/1/0` y `10.0.0.13` a `Serial0/1/1` |
| 4 | Ping a un servidor sin respuesta | El servidor no tenía su IP estática configurada | Configurar IP, máscara, puerta de enlace y DNS en cada servidor, y comprobar con `ping` y `show ip arp` en R6 |
| 5 | Error al crear la ACL | En una ACL extendida falta el protocolo, `deny ip ...` | Escribir `access-list 101 deny ip 192.168.10.0 0.0.0.255 any` |
| 6 | Ping entre la Red 5 y la Red 2 con `Request timed out` | Los PC de la Red 2 seguían en configuración **estática y sin IP**, y no existía ningún equipo con la IP probada | Poner los PC en DHCP y probar con una IP real |
| 7 | `Destination host unreachable` desde la Red 5, con respuesta del propio R4 | R4 estaba casi sin configurar: sin IP en la LAN, sin el enlace con R3 y sin rutas | Repetir su configuración y comprobar con `show ip interface brief` y `show ip route` |
| 8 | `Excluded addresses: 2` en `show ip dhcp pool`, que parecía un error | Ese contador cuenta las **líneas** `excluded-address` configuradas y no las direcciones | Comprobar el rango real en `show running-config`, donde la exclusión sí era correcta |
| 9 | Ping desde el router sin `ping` delante daba `Connection timed out` | Al escribir solo la IP, el router intenta una conexión Telnet | Escribir siempre `ping IP` |
| 10 | Los portátiles Wi-Fi no obtienen IP (`DHCP request failed`, IP 169.254.x.x) | La clave WPA2 de algún portátil estaba mal escrita (contenía el texto `WPA2-PSK`) y, además, la distancia al punto de acceso influye en la asociación | Poner la misma clave en el AP y en cada portátil y acercarlos al punto de acceso. **[COMPLETAR: resultado final]** |
 
### Lecciones aprendidas
 
- Comprobar cada enlace con `ping` entre routers antes de probar desde los PC, de cerca a lejos.
- Usar `show ip interface brief`, `show ip route` y `show running-config` para localizar rápido dónde falla la comunicación.
- Guardar la configuración con `write memory` tras cada router y guardar el `.pkt` con frecuencia.
---
 
## Notas finales
 
- Todas las direcciones del proyecto son privadas (RFC 1918): `192.168.0.0/16` para las redes de equipos y `10.0.0.0/8` para los enlaces entre routers.
- No hay direcciones IP duplicadas ni subredes solapadas.
- El proyecto debe abrirse directamente desde el archivo `redes2.pkt`.
 