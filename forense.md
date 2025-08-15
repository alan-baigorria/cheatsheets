# ANALISIS FORENSE CHEATSHEET

| **Formatos** | |
|--------------|--|
| `.E01` | Formato **EnCase Evidence File** usado en informática forense para almacenar imágenes de disco con compresión y metadatos de la adquisición (hashes, información del caso, notas). |
| `.mem` | Archivo de **volcado de memoria RAM** usado para análisis forense en vivo; contiene procesos, claves, conexiones, y otros datos volátiles. |

| **Comandos – Verificar evidencia** | |
|------------------------------------|--|
| `sha1sum archivo.E01` | Calcula el hash SHA1 del archivo y lo compara con el valor original para verificar integridad. |
| `ewf` | **Enhanced Write Filter** — evita modificaciones en medios durante análisis. |
| `ewfverify archivo.E01` | Verifica que la imagen `.E01` no esté corrupta y que su hash coincida con el original. |
| `ewfinfo archivo.E01` | metadatos de la imagen antes de montarla. |
| `stat archivo` | ver fechas de modificación, acceso y creación. |

| **Archivo** | **Ubicación** | **Importancia** | **Descripción / Uso en Forense** |
|-------------|---------------|-----------------|-----------------------------------|
| `auth.log` | `/var/log/auth.log` | ⭐⭐⭐⭐⭐ | Registra autenticaciones exitosas y fallidas, intentos de `sudo`, cambios de usuario, y accesos remotos (SSH). |
| `secure` | `/var/log/secure` *(algunas distros)* | ⭐⭐⭐⭐⭐ | Similar a `auth.log`, usado en RedHat/CentOS para registrar autenticaciones. |
| `wtmp` | `/var/log/wtmp` | ⭐⭐⭐⭐⭐ | Registro binario de todos los inicios y cierres de sesión (usado con `last`). |
| `btmp` | `/var/log/btmp` | ⭐⭐⭐⭐ | Registro binario de intentos de inicio de sesión fallidos (usado con `lastb`). |
| `.bash_history` | `/home/<usuario>/.bash_history` | ⭐⭐⭐⭐ | Comandos ejecutados por el usuario en Bash. |
| `.ssh/authorized_keys` | `/home/<usuario>/.ssh/authorized_keys` | ⭐⭐⭐⭐ | Claves SSH autorizadas para el usuario. |
| `.ssh/known_hosts` | `/home/<usuario>/.ssh/known_hosts` | ⭐⭐⭐ | Servidores a los que el usuario se conectó por SSH. |
| `syslog` | `/var/log/syslog` | ⭐⭐⭐ | Registro general del sistema: eventos de servicios, errores y advertencias. |
| `messages` | `/var/log/messages` *(algunas distros)* | ⭐⭐⭐ | Registro general del sistema (equivalente a `syslog` en Debian/Ubuntu). |
| `kern.log` | `/var/log/kern.log` | ⭐⭐ | Mensajes y eventos específicos del kernel. |
| `boot.log` | `/var/log/boot.log` | ⭐⭐ | Mensajes de inicio de servicios y procesos en el arranque. |
| `journal` | `/var/log/journal/` | ⭐⭐ | Registros binarios del **systemd journal** (equivalente ampliado de syslog). |
| `history.log` | `/var/log/apt/history.log` | ⭐⭐ | Lista instalaciones, actualizaciones y eliminaciones de paquetes con `apt`. |




| **Comandos – Montar y analizar** | |
|----------------------------------|--|
| `mkdir log1 phy1` | Crea directorios para el montaje de la evidencia. |
| `sudo ewfmount archivo.E01 phy1/` | Monta la imagen `.E01` como dispositivo virtual en `phy1/`. |
| `sudo ls phy1` | Lista el contenido del montaje virtual. |
| `sudo mount -o -ro,norecovery,loop log1/` | Monta el sistema de archivos en modo **solo lectura** para evitar alteraciones. |
| `sudo ls log1/` | Lista los archivos del sistema montado. |
| `sudo chroot log1/` | Cambia el entorno raíz al sistema montado para operar como si fuera el SO original. |
| `cd /var/log/` | Accede al directorio de logs del sistema. |
| `journalctl --list-boots` | Muestra el historial de arranques del sistema. **Nota:** `0` es el último boot. |
| `cat /apt/history.log` | Muestra historial de paquetes instalados/eliminados con apt. |
| `cat history.log \| grep -i -B1 chrome` | Busca si se descargó/instaló Chrome y muestra 1 línea anterior. |
| `cat history.log \| grep -i -A1 josesito` | Busca acciones del usuario `josesito` y muestra 1 línea siguiente. |
| `cat history.log \| grep -i -A3 josesito` | Igual que anterior pero muestra 3 líneas siguientes. |
| `cat auth.log` | Registros de autenticación y comandos ejecutados con privilegios. |
| `cat dmesg* \| grep -i uuid` | Busca el UUID en logs de arranque para documentarlo. |


## Celulares: 
| Herramienta | Plataforma | Descripción breve |
|-------------|------------|-------------------|
| **iLEAPP** (iOS Logs, Events, And Plist Parser) | iOS / iPadOS | Analiza artefactos forenses de dispositivos Apple. Extrae y parsea bases de datos, logs, archivos plist y otros, generando reportes HTML/JSON con actividad de apps, ubicación, redes y eventos del sistema. |
| **ALEAPP** (Android Logs, Events, And Protobuf Parser) | Android | Analiza artefactos forenses de dispositivos Android. Procesa bases de datos SQLite, logs, XML/JSON/Protobuf y genera reportes HTML/JSON con historial de ubicaciones, uso de apps, conexiones y eventos del sistema. |




