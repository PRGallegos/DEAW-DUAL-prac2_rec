# 📘 Configuración de DNS Maestro-Esclavo con Vagrant

Este proyecto configura un sistema DNS maestro-esclavo en dos máquinas virtuales Debian utilizando Bind9.
Se implementan zonas maestras y esclavas para la resolución directa e inversa de nombres dentro de la red privada `192.168.57.0/24`.

# 📑 Descripción General

El entorno utiliza Vagrant para configurar dos servidores DNS:

1. DNSA (Maestro): Gestiona las zonas maestras, incluyendo subdominios y zona inversa.

2. DNSB (Esclavo): Sincroniza las zonas maestras desde DNSA mediante transferencia de zona.

Cada máquina virtual está configurada automáticamente para instalar Bind9, copiar los archivos de configuración necesarios y reiniciar el servicio.

# 📝 Requisitos

1. Vagrant instalado en tu máquina anfitriona.
2. VirtualBox u otro proveedor compatible con Vagrant.
3. Conexión a internet para descargar dependencias.

# 🗃️ Estructura del Proyecto

```
PRACTICA2-REC/
├── Vagrantfile             # Configuración de las máquinas virtuales
├── README.md               # Documentación del proyecto
└── files/
    ├── DNSA/               # Configuración del servidor master
    │   ├── named.conf.local
    │   └── config/
    │       ├── db.192.168.57
    │       ├── db.aulas.ies.test
    │       ├── db.departamentos.ies.test
    │       ├── db.ies.test
    │       └── db.informatica.ies.test
    └── DNSB/               # Configuración del servidor slave
        └── named.conf.local
```

# 📝 Configuración de las Máquinas Virtuales

1. DNSA (Maestro)

   - Hostname: dnsa.ies.test
   - IP: 192.168.57.10
   - Roles:
     - Gestiona zonas maestras.
     - Permite transferencia de zona a DNSB.

2. DNSB (Esclavo)
   - Hostname: dnsb.ies.test
   - IP: 192.168.57.11
   - Roles:
     - Sincroniza las zonas maestras desde DNSA.
     - Prueba de resolución directa e inversa.

# 📝 Uso del Entorno

1. Iniciar las Máquinas
   Desde la raíz del proyecto, ejecuta:

   ```
   vagrant up
   ```

   Esto:

   - Descargará y configurará las máquinas virtuales.
   - Instalará Bind9 en cada máquina.
   - Copiará los archivos de configuración en los servidores.
   - Reiniciará Bind9 para aplicar las configuraciones.

2. Verificar Servicios:

   - Confirma que Bind9 está activo en ambas máquinas:

   ```
   vagrant ssh DNSA
   sudo systemctl status bind9
   ```

   ```
   vagrant ssh DNSB
   sudo systemctl status bind9
   ```

3. Probar DNS
   Desde DNSA, realiza estas pruebas (ejemplos):

   Resolución directa:

   ```
   dig @192.168.57.11 server01.informatica.ies.test
   ```

   Resolución inversa:

   ```
   dig -x 192.168.57.11 @192.168.57.10
   ```

# 📌 Archivos de Configuración

    DNSA (Master)
        - named.conf.local: Define las zonas maestras:
            - ies.test, informatica.ies.test, aulas.ies.test, departamentos.ies.test.
            - Zona inversa: 192.168.57.0/24.

    DNSB (Slave)
        - named.conf.local: Define las zonas esclavas sincronizadas con DNSA.

# ❌ Solución de Problemas

1. Problemas de permisos:

```
sudo chown -R bind:bind /etc/bind/zones/
sudo chmod 644 /etc/bind/zones/*
```

2. Servicio Bind9 No Activo
   Reinicia el servicio y revisa los logs:
   ```
   sudo systemctl restart bind9
   sudo tail -f /var/log/syslog
   ```

# 👤 Autor

    Pedro Rodríguez Gallegos
