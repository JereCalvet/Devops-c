# 🗂️ Linux Filesystem Layout – Cheat Sheet

Este documento resume la **estructura de directorios más importantes en Linux**,
qué contiene cada uno y **cuándo se usa**, con mentalidad de sysadmin.

---

## 🧠 Resumen rápido

- /bin → comandos vitales
- /usr/bin → comandos normales
- /usr/sbin → comandos administración
- /usr/local/bin → cosas del admin
- /opt → software externo grande
- /etc → configuración
- /var → datos y logs

---

## 🧠 Idea base

> **Binarios viven en `bin`**  
> **Configuración vive en `/etc`**  
> **Datos y estado viven en `/var`**

---

## 📂 /bin

### Qué es
- Comandos **esenciales** para el sistema
- Necesarios para arrancar y modo rescate

### Ejemplos
- `ls`
- `cp`
- `mv`
- `rm`
- `cat`
- `bash`
- `chmod`

### Notas
- Disponible incluso sin montar `/usr`
- Accesible a todos los usuarios

---

## 📂 /usr/bin

### Qué es
- Comandos de uso normal del sistema
- Programas instalados por la distro

### Ejemplos
- `ssh`
- `vim`
- `git`
- `curl`

---

## 📂 /usr/sbin

### Qué es
- Comandos de **administración**
- Generalmente requieren root o sudo

### Ejemplos
- `useradd`
- `usermod`
- `systemctl`
- `iptables`
- `nginx`

---

## 📂 /usr/local/bin

### Qué es
- Programas instalados **manualmente por el admin**
- No gestionados por el sistema de paquetes

### Ejemplos
- Scripts personalizados
- terraform
- aws-cli
- kubectl

---

## 📂 /opt

### Qué es
- Software grande, autocontenido
- Aplicaciones de vendors o enterprise

### Ejemplos
- /opt/google/
- /opt/oracle/
- /opt/sonarqube/
- /opt/custom-app/
 
---

## 📂 /etc

### Qué es
- **Configuración del sistema**
- Solo archivos de texto

### Ejemplos
- /etc/passwd
- /etc/shadow
- /etc/ssh/sshd_config
- /etc/fstab
- /etc/crontab

---

## 📂 /var

### Qué es
- Datos variables y estado del sistema

### Ejemplos
- /var/log
- /var/mail
- /var/spool
- /var/cache

---

## 🧠 Reglas mentales finales

- Comando no corre → probablemente no está en el PATH
- Configuración rota → mirar `/etc`
- Logs → mirar `/var/log`
- Software instalado a mano → `/usr/local` o `/opt`

---

## 🧪 Comandos útiles

```bash
which <comando>
command -v <comando>
echo $PATH
ls -l /usr/bin | less
```