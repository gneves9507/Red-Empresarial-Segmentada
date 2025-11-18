1. Descripción General del Proyecto

Este proyecto representa la red de una empresa (un gimnasio) que necesita:

Separar invitados de personal administrativo

Garantizar seguridad entre segmentos

Mantener escalabilidad

Permitir servicios como VoIP, WiFi y DHCP

Controlar el tráfico entre VLANs mediante ACLs

La solución implementa Router-on-a-Stick, VLANs, trunking, subinterfaces, DHCP por VLAN, y ACLs restrictivas
diseñadas para aislar completamente las redes internas de los invitados.

2. Topología General

La red está compuesta por:

1 Router ISR actuando como gateway y DHCP server

1 Switch principal (2960-24TT) manejando los trunks

2 Switches secundarios

Switch 1 – VLAN 10 (Invitados)

Switch 2 – VLAN 20 (Administración)

Múltiples Access Points distribuidos en las áreas

Equipos finales

Laptops y móviles en VLAN 10

PCs y teléfonos VoIP en VLAN 20

La topología está organizada como un core central + dos segmentos completamente separados.

3. VLANs Implementadas
VLAN	Nombre	      Red	            Uso
10	Invitados	      10.0.0.0/8	    WiFi de clientes/invitados
20	Administración	192.168.0.0/24	Área administrativa y VoIP

4. Router-on-a-Stick (Subinterfaces)

El router ISR maneja ambas VLAN mediante subinterfaces configuradas en la interfaz física GigabitEthernet 0/0/0, usando encapsulación dot1Q.

Subinterfaces

GigabitEthernet 0/0/0.10

Encapsulation: dot1q 10

IP: 10.0.0.1

Gateway para VLAN 10

GigabitEthernet 0/0/0.20

Encapsulation: dot1q 20

IP: 192.168.0.1

Gateway para VLAN 20

5. Configuración de Switches
🔹 Switch Principal

Fa0/1 → Trunk hacia Switch Invitados

Fa0/2 → Trunk hacia Switch Administración

Gi0/1 → Trunk hacia el Router (Router-on-a-Stick)

🔹 Switch Invitados (VLAN 10)

Puertos Fa0/1 – Fa0/11 → Access en VLAN 10

Acceso exclusivamente a dispositivos WiFi/APs

🔹 Switch Administración (VLAN 20)

Puertos en Access dentro de VLAN 20

Conectividad para:

PCs de recepción

Teléfonos VoIP

Laptop de administración

Access Point del área administrativa

6. Seguridad – ACLs Implementadas

Se aplicaron ACLs para aislar completamente ambas VLANs:

 ACL para VLAN 10 (Invitados)

Permite tráfico hacia Internet (salida)

Bloquea totalmente acceso a 192.168.0.0/24

Restringe acceso lateral entre hosts de VLAN 10

Evita cualquier contacto con la red administrativa

 ACL para VLAN 20 (Administración)

Permite comunicación interna entre hosts administrativos

Permite acceso a servicios necesarios (servidores, VoIP)

Bloquea acceso a la red de invitados (10.0.0.0/8)

Resultado:
Los invitados tienen solo salida a Internet.
Los administrativos están protegidos y no pueden ser accedidos desde la VLAN de invitados.

7. DHCP por VLAN

El router actúa como DHCP Server:

VLAN 10 (Invitados)

Red: 10.0.0.0/8

Gateway: 10.0.0.1

Pool dinámico para todos los APs y clientes móviles

VLAN 20 (Administración)

Red: 192.168.0.0/24

Gateway: 192.168.0.1

Reserva de IPs opcional para PCs administrativas y VoIP

8. Pruebas de Funcionamiento

Incluye en /img las capturas de:

Ping exitoso entre hosts permitidos

9. Contenido del Repositorio

Bloqueo de tráfico entre VLANs

Asignación DHCP

Configuración de ACLs y subinterfaces

Se incluye el archivo .PKT del simulador Cisco Packet Tracer para ver en accion la red. 


10. Cómo Reproducir el Proyecto

Abrir el archivo .pkt con Cisco Packet Tracer

Revisar los archivos de configuración en /config

Activar los dispositivos y realizar las pruebas mostradas en /img

Modificar o extender ACLs/VLANs según sea necesario

