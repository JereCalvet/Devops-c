Day 42: Create a Docker Network

Enunciado:
The Nautilus DevOps team needs to set up several docker environments for different applications. One of the team members has been assigned a ticket where he has been asked to create some docker networks to be used later. Complete the task based on the following ticket description:

a. Create a docker network named as beta on App Server 1 in Stratos DC.

b. Configure it to use macvlan drivers.

c. Set it to use subnet 192.168.0.0/24 and iprange 192.168.0.0/24.

Resolución:
```bash
ssh steve@stapp01
docker network create -d macvlan --subnet 192.168.0.0/24 --ip-range 192.168.0.0/24
docker network ls
```

---

# Docker Networks – Guía práctica

Este documento resume **cómo funcionan las redes en Docker**, los **drivers disponibles**, cuándo usar cada uno y conceptos clave para troubleshooting y diseño.

---

## 1. ¿Qué es Docker Networking?

Docker Networking permite que:

* contenedores se comuniquen entre sí
* contenedores se comuniquen con el host
* contenedores se comuniquen con redes externas

Docker abstrae la red usando **drivers**, cada uno con un modelo distinto.

---

## 2. Drivers de red disponibles en Docker

### 2.1 bridge (default)

**Qué es:**

* Red virtual interna creada por Docker
* NAT hacia la red del host

**Características:**

* IPs privadas (ej: 172.x.x.x)
* Los contenedores se ven entre sí
* Salida a internet vía NAT

**Uso típico:**

* Desarrollo local
* Apps simples
* Docker Compose

**Notas:**

* El host puede hablar con los contenedores
* Los contenedores NO son visibles desde la red física

---

### 2.2 host

**Qué es:**

* El contenedor usa directamente la red del host

**Características:**

* Sin aislamiento de red
* Mismos puertos que el host
* Sin NAT

**Uso típico:**

* Máximo performance
* Casos muy específicos

**Limitaciones:**

* Solo Linux
* Riesgo de conflictos de puertos

---

### 2.3 none

**Qué es:**

* Sin red

**Características:**

* Solo loopback

**Uso típico:**

* Seguridad extrema
* Jobs batch
* Contenedores totalmente aislados

---

### 2.4 overlay

**Qué es:**

* Red distribuida entre múltiples hosts

**Características:**

* Usa VXLAN
* Requiere Docker Swarm (o Kubernetes CNI)

**Uso típico:**

* Clusters
* Microservicios distribuidos

---

### 2.5 macvlan

**Qué es:**

* Conecta contenedores directamente a la red física

**Modelo mental:**

```
Contenedor → NIC del host → Switch → LAN
```

**Características:**

* IP real de la LAN
* MAC propia por contenedor
* No usa NAT

**Requisitos:**

* Interfaz física (parent)
* Switch que soporte múltiples MACs

**Limitaciones importantes:**

* El host NO puede comunicarse con los contenedores por defecto

**Uso típico:**

* Apps legacy
* Requisitos de IP fija
* Integración con infraestructura existente

---

### 2.6 ipvlan

**Qué es:**

* Similar a macvlan, pero comparte MAC

**Diferencias clave con macvlan:**

* Menos carga para el switch
* Mejor compatibilidad

**Uso típico:**

* Data centers grandes
* Kubernetes avanzado

---

## 3. Conceptos clave de red

### Subnet

Define el rango total de la red:

```
192.168.0.0/24
```

---

### IP Range

Define qué parte del subnet puede asignar Docker:

```
192.168.0.50 - 192.168.0.100
```

---

### Gateway

IP por donde sale el tráfico de la red.

---

### Parent Interface (macvlan / ipvlan)

Interfaz física del host sobre la cual se conectan los contenedores.

Ejemplos:

* eth0
* ens33
* enp0s3

Sin parent, macvlan no tiene "cable".

---

## 4. NAT (Network Address Translation)

**Qué es:**

* Traducción de IP privada → IP pública

**En Docker:**

* bridge usa NAT
* macvlan / host NO usan NAT

---

## 5. Comunicación Host ↔ Contenedor

| Driver  | Host → Container | Container → Host |
| ------- | ---------------- | ---------------- |
| bridge  | ✅                | ✅                |
| host    | ✅                | ✅                |
| macvlan | ❌ (por defecto)  | ❌                |
| none    | ❌                | ❌                |

(macvlan se puede resolver con interfaces extra)

---

## 6. Casos de uso rápidos

* **Desarrollo local:** bridge
* **Compose:** bridge
* **Producción simple:** bridge / overlay
* **IPs reales en LAN:** macvlan / ipvlan
* **Performance extrema:** host
* **Aislamiento total:** none

---

## 7. Errores comunes

* Pensar que macvlan es solo "otra bridge"
* Olvidar el parent interface
* Usar macvlan cuando bridge alcanza
* Exponer servicios innecesarios

---

## 8. Frase para recordar

> bridge = NAT
> macvlan = IP real
> overlay = cluster
