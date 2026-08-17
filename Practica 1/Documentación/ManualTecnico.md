# Práctica 1 - Monte Alto y la conectividad
### Redes de Computadoras 2 
### Grupo #5
----



# Configuración de Red – Edificio Izquierdo

---

## Diseño de VLAN del Edificio Izquierdo

| VLAN | Nombre | Red | Gateway |
|------|--------|-----|---------|
| 15 | PRIMARIA_IZQ | 192.178.15.0/24 | 192.178.15.1 |
| 25 | BASICOS_IZQ | 192.178.25.0/24 | 192.178.25.1 |
| 35 | BACHILLERATO_IZQ | 192.178.35.0/24 | 192.178.35.1 |

---

## 2Configuración VTP (Switch3 – Servidor)

**Verificación:**

```
show vtp status
```

Switch3 debe estar en:
- VTP Mode: **Server**
- Domain: **G5**
- Version: **2**

**Creación de VLAN:**

```
conf t
vlan 15
 name PRIMARIA_IZQ
vlan 25
 name BASICOS_IZQ
vlan 35
 name BACHILLERATO_IZQ
end
write
```

---

## Configuración de Troncales

### Switch3 (Switch central del edificio)

Ejemplo de puertos trunk:

```
interface fa0/6
switchport mode trunk

interface fa0/7
switchport mode trunk

interface fa0/8
switchport mode trunk

interface fa0/9
switchport mode trunk

interface fa0/10
switchport mode trunk
```

**Verificación:**

```
show interfaces trunk
```

###  Switch0 (conectado al Router)

```
interface fa0/1
switchport mode trunk
switchport trunk allowed vlan 1,15,25,35
```

---

## Configuración Router-on-a-Stick (Router0)

```
conf t
interface g0/0
no shutdown

interface g0/0.15
encapsulation dot1Q 15
ip address 192.178.15.1 255.255.255.0

interface g0/0.25
encapsulation dot1Q 25
ip address 192.178.25.1 255.255.255.0

interface g0/0.35
encapsulation dot1Q 35
ip address 192.178.35.1 255.255.255.0
end
write
```

**Verificación:**

```
show ip interface brief
```

> Todas deben aparecer **up/up**.

---

## Configuración de Puertos de Acceso

### VLAN 15 – PRIMARIA

Ejemplo (Switch correspondiente):

```
conf t
interface fa0/1
switchport mode access
switchport access vlan 15
spanning-tree portfast
end
write
```

### VLAN 25 – BASICOS

PC Básicos (Switch7):

```
interface fa0/1
switchport mode access
switchport access vlan 25
spanning-tree portfast
```

### VLAN 35 – BACHILLERATO

PC Bachillerato (Switch6):

```
interface fa0/1
switchport mode access
switchport access vlan 35
spanning-tree portfast
```

Laptop Bachillerato (Switch8):

```
interface fa0/1
switchport mode access
switchport access vlan 35
spanning-tree portfast
```

---

## Direcciones IP Asignadas

### PRIMARIA – VLAN 15

| Dispositivo | IP | Máscara | Gateway |
|-------------|----|---------|---------|
| PC Primaria 1 | 192.178.15.10 | 255.255.255.0 | 192.178.15.1 |
| PC Primaria 2 | 192.178.15.20 | 255.255.255.0 | 192.178.15.1 |

### BASICOS – VLAN 25

| Dispositivo | IP | Máscara | Gateway |
|-------------|----|---------|---------|
| PC1 (verde) | 192.178.25.10 | 255.255.255.0 | 192.178.25.1 |
| PC2 | 192.178.25.20 | 255.255.255.0 | 192.178.25.1 |

###  BACHILLERATO – VLAN 35

| Dispositivo | IP | Máscara | Gateway |
|-------------|----|---------|---------|
| PC Bach 1 | 192.178.35.10 | 255.255.255.0 | 192.178.35.1 |
| Laptop | 192.178.35.20 | 255.255.255.0 | 192.178.35.1 |

---

## Verificaciones Realizadas

**VLAN**

```
show vlan brief
```

**Troncales**

```
show interfaces trunk
```

**VTP**

```
show vtp status
```

**STP**

```
show spanning-tree vlan 15
show spanning-tree vlan 25
show spanning-tree vlan 35
```