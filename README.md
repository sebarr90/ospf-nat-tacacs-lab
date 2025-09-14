# ospf-nat-tacacs-lab
Laboratorio de Packet Tracer con configuración de OSPF, NAT/PAT con ACL y autenticación TACACS+. Incluye logging centralizado y rutas hacia Internet simulada.

# OSPF-NAT-TACACS-Lab

Laboratorio de Packet Tracer con configuración de **OSPF**, **NAT/PAT** con ACL y **autenticación TACACS+**.  
Incluye **logging centralizado** y rutas hacia Internet simulada.

---

## 🖧 Topología de la red

- **Routers:** R1, R2, R3, R4  
- **Redes internas:**
  - R1: 172.16.11.0/24, 172.16.21.0/24  
  - R2: 172.16.21.0/24, 172.16.22.0/24, 172.16.32.0/24  
  - R3: 172.16.32.0/24, salida a Internet 208.50.0.0/30  
  - R4: 172.16.12.0/24, 172.16.11.0/24  

- **Objetivo:**  
  - Red interna 172.16.12.0/24 debe tener acceso a Internet mediante NAT/PAT en R3.  
  - R1 y R2 deben autenticarse mediante TACACS+.  
  - Todos los routers envían logs al servidor Syslog (172.16.12.20).

---

## 🔁 Configuración OSPF

- Configuración general de cada router:

```text
R1:
router ospf 1
 router-id 1.1.1.1
 log-adjacency-changes
 passive-interface default
 no passive-interface Serial0/0/0
 no passive-interface Serial0/0/1
 network 172.16.21.0 0.0.0.255 area 0
 network 172.16.11.0 0.0.0.255 area 0

R2:
router ospf 1
 router-id 2.2.2.2
 passive-interface default
 no passive-interface Serial0/0/0
 no passive-interface Serial0/0/1
 network 172.16.21.0 0.0.0.255 area 0
 network 172.16.32.0 0.0.0.255 area 0
 network 172.16.22.0 0.0.0.255 area 0

R3:
router ospf 1
 router-id 3.3.3.3
 passive-interface default
 no passive-interface Serial0/0/0
 network 172.16.32.0 0.0.0.255 area 0
 network 208.50.0.0 0.0.0.3 area 0

R4:
router ospf 1
 router-id 4.4.4.4
 passive-interface default
 no passive-interface Serial0/0/0
 network 172.16.11.0 0.0.0.255 area 0
 network 172.16.12.0 0.0.0.255 area 0
