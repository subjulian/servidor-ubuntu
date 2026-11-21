# Problemas encontrados y cómo se resolvieron

| Síntoma | Causa | Solución |
|---|---|---|
| La VM no arrancaba (`VERR_SVM_DISABLED`) | AMD-V (SVM) desactivado en el BIOS | Activar la virtualización en el BIOS |
| `apt` fallaba con `Temporary failure resolving` y `ping` con `Network is unreachable` | La interfaz `enp0s3` tenía solo IPv6, sin IPv4 | Diagnosticar con `ip -br a` y `networkctl status enp0s3` |
| `cat` daba `Permission denied` al leer la configuración de red | El archivo pertenece a `root` con permisos `-rw-------` | Usar `sudo` |
| Faltaba la clave DHCP en netplan | `/etc/netplan/00-installer-config.yaml` no tenía `dhcp4` | Agregar `dhcp4: true` dentro de `enp0s3`, alineado con `match` y `set-name`, y probar con `sudo netplan try` |
| En modo puente (Wi‑Fi) la VM no obtenía IPv4 | Probable limitación del puente sobre tarjetas inalámbricas (no se confirmó del todo) | Usar NAT con reenvío de puertos |
| `sudo group add prueba` daba `command not found` | Error de tecleo: el comando es `groupadd`, una sola palabra | Corregir el comando |
| `ssh-keygen` se ejecutó dentro de la VM | La ventana de CMD estaba conectada por SSH a la VM, y el prompt era `julian@Ubuntu:~$` | Cancelar con `Ctrl+C`, cerrar la sesión con `exit` y generar la llave desde el prompt `C:\Users\...>` de Windows |
| SSH seguía pidiendo la contraseña con la llave instalada | La línea de `authorized_keys` no incluía `ssh-ed25519` (el archivo pesaba 69 bytes) y los permisos eran `-rw-rw-r--` | Pegar la línea completa (102 bytes) y aplicar `chmod 600` |
| `Connection refused` al probar `ssh -p 2222` desde dentro de la VM | Dentro de la VM, `127.0.0.1` es la propia VM, donde SSH escucha en el 22 y no en el 2222 | Hacer las pruebas de acceso desde Windows, no desde la sesión de la VM |
| `syntax error near unexpected token ')'` al crear `~/prueba` | Se copió el comando con un `)` y un `:` sobrantes | Reescribir el comando sin los caracteres extra |
| `sudo ufw delete 8080` daba `Could not find rule '8080'` | Con `delete`, un número se interpreta como número de regla, no como puerto | Usar `sudo ufw delete allow 8080/tcp` (borra v4 y v6) o el número de regla de `status numbered` |
| `jail.local` solo con `maxretry` y `bantime`, sin encabezado | Las opciones deben ir bajo una sección INI como `[sshd]`; sin ella fail2ban no arranca | Escribir `[sshd]` y `enabled = true` al principio |
| `fail2ban-client get sshd banaction` daba `Invalid command` | `banaction` es una opción del archivo, no tiene `get` en el cliente | Verificar con `get sshd actions` y por la regla de ufw tras un baneo |
| `get sshd bantime` seguía respondiendo 60 tras editar | El servicio no se había reiniciado con el cambio | `sudo systemctl restart fail2ban` y volver a verificar (respondió 600) |
| `udo: command not found` y `Sudo: command not found` | Errores de tecleo: `udo` sin la `s`, y `Sudo` con mayúscula (Linux distingue mayúsculas) | Escribir `sudo` en minúsculas |
| `apt list --upgradable \| wc -l` daba 6 con 5 paquetes pendientes | La salida incluye la línea de encabezado `Listing...` y el aviso de que `apt` no es estable para scripts | Restar la línea de encabezado: el aviso no es un error |
| `ls /var/log/unattended-upgrades/` daba `Permission denied` | El directorio pertenece a `root` | Usar `sudo` para leerlo |
| `apt upgrade` dejaba 5 paquetes sin actualizar | *Phasing*: Ubuntu libera esas actualizaciones por etapas | No forzar; se instalarán solas |
| `grep -v '^//'` no mostraba `Automatic-Reboot` | La opción viene comentada (`//`) y el filtro quitaba esas líneas | Buscarla con `grep -n "Automatic-Reboot"` |
| `sshd -T` daba `no hostkeys available -- exiting` | Se ejecutó sin `sudo`: las claves del host solo las lee `root` | Ejecutar `sudo sshd -T` |
| `show details PKGS-7392` daba `Command 'show' not found` y sugería instalar paquetes | Faltaba el `sudo lynis` del principio, y Ubuntu buscó un programa llamado `show` | Escribir el comando completo `sudo lynis show details PKGS-7392`; no instalar lo sugerido |


---
[← Volver al índice](../README.md)
