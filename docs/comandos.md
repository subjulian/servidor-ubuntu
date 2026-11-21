# Comandos principales

| Comando | Para qué sirve |
|---|---|
| `ip -br a` | Ver interfaces de red y sus IP |
| `ping -c 3 8.8.8.8` | Probar conectividad a internet |
| `sudo cat /etc/netplan/*.yaml` | Leer la configuración de red (requiere `sudo`) |
| `sudo nano /etc/netplan/00-installer-config.yaml` | Editar la configuración (`Ctrl+O`, Enter, `Ctrl+X`) |
| `sudo netplan try` | Aplicar la configuración con reversión automática |
| `sudo apt update` | Actualizar el índice de paquetes |
| `ssh -p 2222 usuario@127.0.0.1` | Conectar por SSH desde Windows |
| `passwd` | Cambiar la contraseña del usuario actual |
| `sudo adduser usuario` | Crear un usuario normal con su carpeta personal |
| `sudo usermod -a -G sudo usuario` | Agregar un usuario al grupo `sudo` sin quitarle otros grupos |
| `id usuario` | Ver UID, grupo primario y grupos suplementarios |
| `getent group nombre` | Comprobar si un grupo existe |
| `ssh-keygen -t ed25519` | Generar un par de llaves (en el equipo cliente) |
| `chmod 600 archivo` | Dejar un archivo legible y editable solo por su dueño |
| `sudo sshd -t` | Validar la sintaxis de la configuración de SSH sin reiniciar |
| `sudo sshd -T` | Ver la configuración efectiva de SSH (requiere `sudo`) |
| `sudo systemctl restart ssh` | Aplicar cambios en el servidor SSH |
| `ssh -o PubkeyAuthentication=no ...` | Probar que el servidor rechaza la contraseña |
| `ssh -p 2222 -N -L 9000:127.0.0.1:22 usuario@127.0.0.1` | Abrir un túnel SSH (prueba de `AllowTcpForwarding`) |
| `sudo ss -tlnp` | Ver qué procesos escuchan en qué puertos |
| `sudo ufw status verbose` | Ver estado, políticas y reglas de ufw |
| `sudo ufw status numbered` | Ver las reglas numeradas |
| `sudo ufw show added` | Ver las reglas agregadas aunque ufw esté apagado |
| `sudo ufw default deny incoming` | Denegar por defecto el tráfico entrante (ídem `allow outgoing`) |
| `sudo ufw allow 22/tcp` | Permitir un puerto TCP |
| `sudo ufw enable` | Activar el firewall |
| `sudo ufw delete allow 8080/tcp` | Borrar una regla por su especificación |
| `sudo ufw delete N` | Borrar una regla por su número (los números se reordenan tras cada borrado) |
| `python3 -m http.server 8080` | Levantar un servidor web de prueba |
| `sudo apt install fail2ban` | Instalar fail2ban |
| `sudo fail2ban-client status [jail]` | Ver jails activos o el estado de uno (fallos, baneos, IP baneadas) |
| `sudo fail2ban-client get sshd bantime` | Ver un valor efectivo del jail (siempre en segundos) |
| `sudo fail2ban-client set sshd unbanip IP` | Quitar el baneo de una IP |
| `ls /etc/fail2ban/action.d` | Ver las acciones de baneo disponibles (por ejemplo `ufw.conf`) |
| `apt list --upgradable` | Ver paquetes con actualización pendiente |
| `sudo apt upgrade` | Instalar las versiones nuevas de los paquetes instalados |
| `sudo apt autoremove` | Quitar paquetes que ya no hacen falta (por ejemplo, un kernel anterior) |
| `uname -r` | Ver la versión del kernel en ejecución |
| `ls /var/run/reboot-required` | Comprobar si el sistema pide reinicio |
| `sudo unattended-upgrade --dry-run` | Simular las actualizaciones automáticas sin cambiar nada |
| `systemctl list-timers 'apt-daily*'` | Ver cuándo corren los temporizadores de actualización |
| `sudo ss -tulpn` | Listar puertos en escucha con su proceso |
| `systemctl list-units --type=service --state=running` | Ver servicios activos |
| `systemctl list-dependencies --reverse servicio` | Ver qué depende de un servicio |
| `sudo systemctl disable --now servicio` | Apagar un servicio y evitar que arranque al iniciar |
| `df -h` | Ver el espacio en disco |
| `sudo apt install lynis` / `lynis --version` | Instalar lynis y ver su versión |
| `sudo lynis audit system` | Ejecutar la auditoría completa |
| `sudo lynis show details CÓDIGO` | Ver qué comprobó una prueba y por qué falló |
| `sudo grep -c '^suggestion\[\]' /var/log/lynis-report.dat` | Contar las sugerencias (con `warning` para los avisos) |
| `dpkg -l \| grep '^rc'` | Ver paquetes removidos con configuración residual |
| `sudo apt purge paquete` | Quitar un paquete junto con su configuración |


---
[← Volver al índice](../README.md)
